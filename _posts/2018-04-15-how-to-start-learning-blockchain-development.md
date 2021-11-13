---
layout: post
title:  "How To Start Learning Blockchain Development"
tags: [ Tips, Development  ]
featured_image_thumbnail:
featured_image: assets/images/posts/2018/3.jpg
featured: true
author: nancy
---

A blockchain, originally block chain, is a continuously growing list of records, called blocks, which are linked and secured using cryptography. Each block typically contains a hash pointer as a link to a previous block, a timestamp and transaction data. By design, a blockchain is inherently resistant to modification of the data. It is *"an open, distributed ledger that can record transactions between two parties efficiently and in a verifiable and permanent way"*. 

<!--more-->

For use as a distributed ledger, a blockchain is typically managed by a peer-to-peer network collectively adhering to a protocol for validating new blocks. Once recorded, the data in any given block cannot be altered retroactively without the alteration of all subsequent blocks, which requires collusion of the network majority.

Blockchains are secure by design and are an example of a distributed computing system with high Byzantine fault tolerance. Decentralized consensus has therefore been achieved with a blockchain. This makes blockchains potentially suitable for the recording of events, medical records, and other records management activities, such as identity management, transaction processing, documenting provenance, food traceability or voting.

## Code blocks, highlighted with [Prism.js](http://prismjs.com/index.html)

<pre><code class="language-markup">&lt;!DOCTYPE html&gt;
&lt;html lang="en"&gt;
&lt;head&gt;
	&lt;meta charset="utf-8" /&gt;
	&lt;title&gt;Prism&lt;/title&gt;
	&lt;link rel="stylesheet" href="style.css" /&gt;
&lt;/head&gt;
&lt;body&gt;
	&lt;header&gt;
		&lt;div class="intro" data-src="templates/header-main.html" data-type="text/html"&gt;&lt;/div&gt;
		&lt;ul id="features"&gt;
			&lt;li&gt;Dead simple&lt;/li&gt;
			&lt;li&gt;Light as a feather&lt;/li&gt;
			&lt;li&gt;Extensible&lt;/li&gt;
		&lt;/ul&gt;
	&lt;/header&gt;
	&lt;section id="features-full" class="language-markup"&gt;
		&lt;h1&gt;Full list of features&lt;/h1&gt;
		&lt;ul&gt;
			&lt;li&gt;&lt;strong&gt;Only 2KB&lt;/strong&gt; minified &amp; gzipped (core). Each language definition adds roughly 300-500 bytes.&lt;/li&gt;
			&lt;li&gt;Very easy to extend without modifying the code, due to Prism’s &lt;a href="#plugins"&gt;plugin architecture&lt;/a&gt;. Multiple hooks are scattered throughout the source.&lt;/li&gt;
			&lt;li&gt;Very easy to &lt;a href="extending.html#language-definitions"&gt;define new languages&lt;/a&gt;. Only thing you need is a good understanding of regular expressions&lt;/li&gt;
		&lt;/ul&gt;
	&lt;/section&gt;
	&lt;footer data-src="templates/footer.html" data-type="text/html"&gt;&lt;/footer&gt;
&lt;/body&gt;
&lt;/html&gt;
</code></pre>

<pre><code class="language-javascript">var Token = _.Token = function(type, content, alias, matchedStr, greedy) {
	this.type = type;
	this.content = content;
	this.alias = alias;
	// Copy of the full string this token was created from
	this.length = (matchedStr || "").length|0;
	this.greedy = !!greedy;
};</code></pre>

<pre><code class="language-css">@import url(http://fonts.googleapis.com/css?family=Arvo);

/* Styles */

body {
	font: 100%/1.5 Questrial, sans-serif;
	tab-size: 4;
	hyphens: auto;
}

a {
	color: inherit;
}

section h1 {
	font-size: 250%;
}

section h1,
#features li strong,
header h2,
footer p {
	font: 100% Rockwell, Arvo, serif;
}</code></pre>

The first blockchain was conceptualized in 2008 by an anonymous person or group known as Satoshi Nakamoto and implemented in 2009 as a core component of bitcoin where it serves as the public ledger for all transactions. The invention of the blockchain for bitcoin made it the first digital currency to solve the double spending problem without the need of a trusted authority or central server. The bitcoin design has been the inspiration for other applications.

## History

The first work on a cryptographically secured chain of blocks was described in 1991 by Stuart Haber and W. Scott Stornetta. In 1992, Bayer, Haber and Stornetta incorporated Merkle trees to the design, which improved its efficiency by allowing several documents to be collected into one block.

![](https://images.unsplash.com/photo-1451187580459-43490279c0fa?ixlib=rb-0.3.5&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=1080&fit=max&ixid=eyJhcHBfaWQiOjExNzczfQ&s=163e3ea37d2c0fda3f586d5552752f59#wide)

>”Bitcoin is the beginning of something great: a currency without a government, something necessary and imperative.” <cite>– Peter Thiel, Co-Founder of PayPal –</cite>

The first blockchain was conceptualised by an anonymous person or group known as Satoshi Nakamoto in 2008. It was implemented the following year as a core component of the digital currency bitcoin, where it serves as the public ledger for all transactions on the network. By using a blockchain, bitcoin became the first digital currency to solve the double spending problem without requiring a trusted administrator and has been the inspiration for many additional applications.

## Structure

A blockchain is a decentralized and distributed digital ledger that is used to record transactions across many computers so that the record cannot be altered retroactively without the alteration of all subsequent blocks and the collusion of the network. This allows the participants to verify and audit transactions inexpensively. A blockchain database is managed autonomously using a peer-to-peer network and a distributed timestamping server. They are authenticated by mass collaboration powered by collective self-interests. The result is a robust workflow where participants' uncertainty regarding data security is marginal. The use of a blockchain removes the characteristic of infinite reproducibility from a digital asset. It confirms that each unit of value was transferred only once, solving the long-standing problem of double spending. 

Blockchains have been described as a value-exchange protocol. This blockchain-based exchange of value can be completed more quickly, more safely and more cheaply than with traditional systems. A blockchain can assign title rights because it provides a record that compels offer and acceptance. [(Source)](https://en.wikipedia.org/wiki/Blockchain)
