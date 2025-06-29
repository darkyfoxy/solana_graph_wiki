# What is an RPC Node?

In order to interact with the Solana blockchain, applications use something called an **RPC node** — short for **Remote Procedure Call node**.

Every time Solana Graph fetches account data, transaction history, or program state — it does so **by sending requests to an RPC node**. This is the main communication gateway between the app and the blockchain.

---

## Why Do You Need One?

Solana is a decentralized network, but to work with it, you need to connect to a **RPC node** that can handle your requests — like getting account info or transaction history.

This is exactly what Solana Graph does behind the scenes. All requests in Solana Graph go through an RPC node.

---

## ⚠️ Don’t Use the Default RPC

Using the **official public Solana RPC** (like `https://api.mainnet-beta.solana.com`) puts **unnecessary load on shared infrastructure**.  
It may also cause **rate limits** or **incomplete data** due to performance throttling.

That’s why we **highly recommend** using a **private or free RPC endpoint** from trusted providers. This improves your experience and keeps the public node healthy. 🙏

---

## ✅ Free RPC Providers

You can get a free Solana RPC endpoint from these platforms:


- <a href="https://helius.dev" target="_blank" >helius.dev</a>
- <a href="https://quicknode.com" target="_blank" >quicknode.com</a>
- <a href="https://alchemy.com/" target="_blank" >alchemy.com</a>
- <a href="https://chainstack.com" target="_blank" >chainstack.com</a>
- <a href="https://syndica.io/" target="_blank" >syndica.io</a>
- <a href="https://getblock.io/" target="_blank" >getblock.io</a>

Most of these services allow you to create a **RPC URL** in just a few clicks.

---

## How to Use Your RPC in Solana Graph

Once you have your RPC URL:

1. Open [**solanagraph.com**](https://solanagraph.com/)  
2. Paste your custom RPC endpoint into the **RPC URL** field in the top bar  
3. Click the **"Set RPC URL"** button next to it

That’s it! Solana Graph will now use your node for all requests. 🚀

> Using a dedicated RPC improves speed, stability, and accuracy.

---

Thank you for helping us keep things fast, efficient! 💚