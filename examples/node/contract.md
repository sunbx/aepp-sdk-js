<!DOCTYPE html>

<html>
<head>
  <title>Simple Sophia Contract Compiler</title>
  <meta http-equiv="content-type" content="text/html; charset=UTF-8">
  <meta name="viewport" content="width=device-width, target-densitydpi=160dpi, initial-scale=1.0; maximum-scale=1.0; user-scalable=0;">
  <link rel="stylesheet" media="all" href="../../docco.css" />
</head>
<body>
  <div id="container">
    <div id="background"></div>
    
      <ul id="jump_to">
        <li>
          <a class="large" href="javascript:void(0);">Jump To &hellip;</a>
          <a class="small" href="javascript:void(0);">+</a>
          <div id="jump_wrapper">
          <div id="jump_page_wrapper">
            <div id="jump_page">
              
                
                <a class="source" href="contract.html">
                  examples/node/contract.js
                </a>
              
                
                <a class="source" href="wallet.html">
                  examples/node/wallet.js
                </a>
              
            </div>
          </div>
        </li>
      </ul>
    
    <ul class="sections">
        
        
        
        <li id="section-1">
            <div class="annotation">
              
              <div class="sswrap ">
                <a class="ss" href="#section-1">&#x00a7;</a>
              </div>
              
            </div>
            
            <div class="content"><div class='highlight'><pre><span class="hljs-meta">#!/usr/bin/env node</span></pre></div></div>
            
        </li>
        
        
        <li id="section-2">
            <div class="annotation">
              
              <div class="sswrap ">
                <a class="ss" href="#section-2">&#x00a7;</a>
              </div>
              <h1 id="simple-sophia-contract-compiler">Simple Sophia Contract Compiler</h1>
<p>This script demonstrates how to</p>
<ul>
<li>deal with the different phases of compiling Sophia contracts to bytecode,</li>
<li>deploying the bytecode to get a callable contract address and ultimately,</li>
<li>invoke the deployed contract on the æternity blockchain.</li>
</ul>

            </div>
            
            <div class="content"><div class='highlight'><pre><span class="hljs-comment">/*
 * ISC License (ISC)
 * Copyright (c) 2021 aeternity developers
 *
 *  Permission to use, copy, modify, and/or distribute this software for any
 *  purpose with or without fee is hereby granted, provided that the above
 *  copyright notice and this permission notice appear in all copies.
 *
 *  THE SOFTWARE IS PROVIDED &quot;AS IS&quot; AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH
 *  REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY
 *  AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT,
 *  INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM
 *  LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR
 *  OTHER TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR
 *  PERFORMANCE OF THIS SOFTWARE.
 */</span></pre></div></div>
            
        </li>
        
        
        <li id="section-3">
            <div class="annotation">
              
              <div class="sswrap ">
                <a class="ss" href="#section-3">&#x00a7;</a>
              </div>
              <p>We’ll need the main client module <code>Sdk</code> in the <code>Universal</code> flavor from the SDK.</p>

            </div>
            
            <div class="content"><div class='highlight'><pre><span class="hljs-keyword">const</span> { <span class="hljs-attr">Universal</span>: Sdk, Node, MemoryAccount } = <span class="hljs-built_in">require</span>(<span class="hljs-string">&#x27;../../dist/aepp-sdk&#x27;</span>)</pre></div></div>
            
        </li>
        
        
        <li id="section-4">
            <div class="annotation">
              
              <div class="sswrap ">
                <a class="ss" href="#section-4">&#x00a7;</a>
              </div>
              <p>Define some constants</p>

            </div>
            
            <div class="content"><div class='highlight'><pre><span class="hljs-keyword">const</span> CONTRACT_CODE = <span class="hljs-string">`
contract Multiplier =
  record state = { factor: int }
  entrypoint init(f : int) : state = { factor = f }
  entrypoint multiplyBy(x : int) = x * state.factor
`</span>
<span class="hljs-keyword">const</span> ACCOUNT_KEYPAIR = {
  <span class="hljs-attr">publicKey</span>: <span class="hljs-string">&#x27;ak_2dATVcZ9KJU5a8hdsVtTv21pYiGWiPbmVcU1Pz72FFqpk9pSRR&#x27;</span>,
  <span class="hljs-attr">secretKey</span>: <span class="hljs-string">&#x27;bf66e1c256931870908a649572ed0257876bb84e3cdf71efb12f56c7335fad54d5cf08400e988222f26eb4b02c8f89077457467211a6e6d955edb70749c6a33b&#x27;</span>
}
<span class="hljs-keyword">const</span> NODE_URL = <span class="hljs-string">&#x27;https://testnet.aeternity.io&#x27;</span></pre></div></div>
            
        </li>
        
        
        <li id="section-5">
            <div class="annotation">
              
              <div class="sswrap ">
                <a class="ss" href="#section-5">&#x00a7;</a>
              </div>
              <p>Generate the account instance based on the keypair</p>

            </div>
            
            <div class="content"><div class='highlight'><pre><span class="hljs-keyword">const</span> account = MemoryAccount({ <span class="hljs-attr">keypair</span>: ACCOUNT_KEYPAIR });</pre></div></div>
            
        </li>
        
        
        <li id="section-6">
            <div class="annotation">
              
              <div class="sswrap ">
                <a class="ss" href="#section-6">&#x00a7;</a>
              </div>
              <p>Most methods in the SDK return <em>Promises</em>, so the recommended way of
dealing with subsequent actions is running them one by one using <code>await</code>.</p>

            </div>
            
            <div class="content"><div class='highlight'><pre>(<span class="hljs-keyword">async</span> () =&gt; {
  <span class="hljs-keyword">const</span> node = <span class="hljs-keyword">await</span> Node({ <span class="hljs-attr">url</span>: NODE_URL })</pre></div></div>
            
        </li>
        
        
        <li id="section-7">
            <div class="annotation">
              
              <div class="sswrap ">
                <a class="ss" href="#section-7">&#x00a7;</a>
              </div>
              <p><code>Sdk</code> itself is asynchronous as it determines the node’s version and
rest interface automatically. Only once the Promise is fulfilled, we know
we have a working <code>Sdk</code> instance. Please take note <code>Sdk</code> is not a constructor but
a factory, which means it’s <em>not</em> invoked with <code>new</code>.</p>

            </div>
            
            <div class="content"><div class='highlight'><pre>  <span class="hljs-keyword">const</span> sdk = <span class="hljs-keyword">await</span> Sdk({
    <span class="hljs-attr">nodes</span>: [{ <span class="hljs-attr">name</span>: <span class="hljs-string">&#x27;testnet&#x27;</span>, <span class="hljs-attr">instance</span>: node }],
    <span class="hljs-attr">compilerUrl</span>: <span class="hljs-string">&#x27;https://compiler.aepps.com&#x27;</span>,
    <span class="hljs-attr">accounts</span>: [account]
  })</pre></div></div>
            
        </li>
        
        
        <li id="section-8">
            <div class="annotation">
              
              <div class="sswrap ">
                <a class="ss" href="#section-8">&#x00a7;</a>
              </div>
              <p><code>contractCompile</code> takes a raw Sophia contract in string form and sends it
off to the HTTP compiler for bytecode compilation. In the future this will be done
without talking to the node, but requiring a bytecode compiler
implementation directly in the SDK.</p>

            </div>
            
            <div class="content"><div class='highlight'><pre>  <span class="hljs-keyword">const</span> bytecode = <span class="hljs-keyword">await</span> sdk.contractCompile(CONTRACT_CODE)
  <span class="hljs-built_in">console</span>.log(<span class="hljs-string">`Obtained bytecode <span class="hljs-subst">${bytecode.bytecode}</span>`</span>)</pre></div></div>
            
        </li>
        
        
        <li id="section-9">
            <div class="annotation">
              
              <div class="sswrap ">
                <a class="ss" href="#section-9">&#x00a7;</a>
              </div>
              <p>Invoking <code>deploy</code> on the bytecode object will result in the contract
being written to the chain, once the block has been mined.
Sophia contracts always have an <code>init</code> method which needs to be invoked,
even when the contract’s <code>state</code> is <code>unit</code> (<code>()</code>). The arguments to
<code>init</code> have to be provided at deployment time and will be written to the
block as well, together with the contract’s bytecode.</p>

            </div>
            
            <div class="content"><div class='highlight'><pre>  <span class="hljs-keyword">const</span> deployed = <span class="hljs-keyword">await</span> bytecode.deploy([<span class="hljs-string">&#x27;5&#x27;</span>])
  <span class="hljs-built_in">console</span>.log(<span class="hljs-string">`Contract deployed at <span class="hljs-subst">${deployed.address}</span>`</span>)</pre></div></div>
            
        </li>
        
        
        <li id="section-10">
            <div class="annotation">
              
              <div class="sswrap ">
                <a class="ss" href="#section-10">&#x00a7;</a>
              </div>
              <p>Once the contract has been successfully mined, we can attempt to invoke
any public function defined within it. The miner who found the next block
will not only be rewarded a fixed amount, but also an amount depending on
the amount of gas spend.</p>

            </div>
            
            <div class="content"><div class='highlight'><pre>  <span class="hljs-keyword">const</span> call = <span class="hljs-keyword">await</span> deployed.call(<span class="hljs-string">&#x27;multiplyBy&#x27;</span>, [<span class="hljs-string">&#x27;7&#x27;</span>])
  <span class="hljs-built_in">console</span>.log(<span class="hljs-string">`Contract call transaction hash <span class="hljs-subst">${call.hash}</span>`</span>)</pre></div></div>
            
        </li>
        
        
        <li id="section-11">
            <div class="annotation">
              
              <div class="sswrap ">
                <a class="ss" href="#section-11">&#x00a7;</a>
              </div>
              <p>The execution result, if successful, will be an FATE-encoded result value.
We are using HTTP compiler to decode the result value.</p>

            </div>
            
            <div class="content"><div class='highlight'><pre>  <span class="hljs-built_in">console</span>.log(<span class="hljs-string">`Execution result: <span class="hljs-subst">${<span class="hljs-keyword">await</span> call.decode()}</span>`</span>)
})()</pre></div></div>
            
        </li>
        
    </ul>
  </div>
</body>
</html>
