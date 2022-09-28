---
layout: post
title:  "NPM Cheat Sheet"
date:   2021-05-25 00:00:00 +0800
categories: javascript npm
---

## Installation
Install using a [node version manager](http://npm.github.io/installation-setup-docs/installing/using-a-node-version-manager.html)
```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
nvm --version
```

Then you can install the latest `node` version and update `npm`
```sh
nvm install node
node -v
npm install npm@latest -g
npm -v
```

## Configuration

<table class="table">
	<colgroup>
		<col width="50%"/>
		<col width="50%"/>
	</colgroup>
	<thead class="thead-light">
		<tr>
			<th>Command</th>
			<th>Description</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>
				<code>npm config list</code>
			</td>
			<td>list configuration</td>
		</tr>
		<tr>
			<td>
				<code>npm config list ls -l | grep strict-ssl</code>
			</td>
			<td>list configuration including all default (and grep strict-ssl  property)</td>
		</tr>
		<tr>
			<td>
				<code>npm config set strict-ssl false</code>
			</td>
			<td>set a property</td>
		</tr>
		<tr>
			<td>
				<code>npm config set registry http://registry.npmjs.org/</code>
			</td>
			<td>set a property registry (defult is https)</td>
		</tr>
		<tr>
			<td>
				<code>npm config rm strict-ssl</code>
			</td>
			<td>delete a property, its default value still applies</td>
		</tr>
	</tbody>
</table>
