# 比特幣閃電網路：可擴展的 off-chain 即時支付 | The Bitcoin Lightning Network: Scalable Off-Chain Instant Payments
![https://github.com/ChenPoWei](https://img.shields.io/badge/Translator-Chen%20Po%20Wei-blue.svg)

Joseph Poon
joseph@lightning.network 

Thaddeus Dryja
rx@awsomnet.org

二零一五年十一月二十日 草案版本 0.5.9.1

## 目錄
- [比特幣閃電網路：可擴展的 off-chain 即時支付 | The Bitcoin Lightning Network: Scalable Off-Chain Instant Payments](#%E6%AF%94%E7%89%B9%E5%B9%A3%E9%96%83%E9%9B%BB%E7%B6%B2%E8%B7%AF%E5%8F%AF%E6%93%B4%E5%B1%95%E7%9A%84-off-chain-%E5%8D%B3%E6%99%82%E6%94%AF%E4%BB%98--the-bitcoin-lightning-network-scalable-off-chain-instant-payments)
  - [目錄](#%E7%9B%AE%E9%8C%84)
  - [摘要 | Abstract](#%E6%91%98%E8%A6%81--abstract)
  - [1 比特幣區塊鏈可擴展性問題 | The Bitcoin Blockchain Scalability Problem](#1-%E6%AF%94%E7%89%B9%E5%B9%A3%E5%8D%80%E5%A1%8A%E9%8F%88%E5%8F%AF%E6%93%B4%E5%B1%95%E6%80%A7%E5%95%8F%E9%A1%8C--the-bitcoin-blockchain-scalability-problem)
  - [2 小額支付通道可以解決可擴展性問題 | A	Network	of	Micropayment	Channels	Can Solve Scalability](#2-%E5%B0%8F%E9%A1%8D%E6%94%AF%E4%BB%98%E9%80%9A%E9%81%93%E5%8F%AF%E4%BB%A5%E8%A7%A3%E6%B1%BA%E5%8F%AF%E6%93%B4%E5%B1%95%E6%80%A7%E5%95%8F%E9%A1%8C--a-network-of-micropayment-channels-can-solve-scalability)
    - [2.1 小額支付通道不要求信託 | Micropayment Channels Do Not Require Trust](#21-%E5%B0%8F%E9%A1%8D%E6%94%AF%E4%BB%98%E9%80%9A%E9%81%93%E4%B8%8D%E8%A6%81%E6%B1%82%E4%BF%A1%E8%A8%97--micropayment-channels-do-not-require-trust)
    - [2.2 通道網路 | A Network of Channels](#22-%E9%80%9A%E9%81%93%E7%B6%B2%E8%B7%AF--a-network-of-channels)
  - [3 雙向支付通道 | Bidirectional Payment Channels](#3-%E9%9B%99%E5%90%91%E6%94%AF%E4%BB%98%E9%80%9A%E9%81%93--bidirectional-payment-channels)
    - [3.1 頻道創建中存在的問題](#31-%E9%A0%BB%E9%81%93%E5%89%B5%E5%BB%BA%E4%B8%AD%E5%AD%98%E5%9C%A8%E7%9A%84%E5%95%8F%E9%A1%8C)
      - [3.1.1 創建無簽署的資金交易 | The Problem of Blame in Channel Creation](#311-%E5%89%B5%E5%BB%BA%E7%84%A1%E7%B0%BD%E7%BD%B2%E7%9A%84%E8%B3%87%E9%87%91%E4%BA%A4%E6%98%93--the-problem-of-blame-in-channel-creation)
      - [3.1.2 來自未簽署交易的消費 | Spending from an Unsigned Transaction](#312-%E4%BE%86%E8%87%AA%E6%9C%AA%E7%B0%BD%E7%BD%B2%E4%BA%A4%E6%98%93%E7%9A%84%E6%B6%88%E8%B2%BB--spending-from-an-unsigned-transaction)
      - [3.1.3 承諾交易：不可執行的結構 | Commitment Transactions: Unenforcible Construction](#313-%E6%89%BF%E8%AB%BE%E4%BA%A4%E6%98%93%E4%B8%8D%E5%8F%AF%E5%9F%B7%E8%A1%8C%E7%9A%84%E7%B5%90%E6%A7%8B--commitment-transactions-unenforcible-construction)
      - [3.1.4 承諾交易：指出禍源 | Commitment Transactions: Ascribing Blame](#314-%E6%89%BF%E8%AB%BE%E4%BA%A4%E6%98%93%E6%8C%87%E5%87%BA%E7%A6%8D%E6%BA%90--commitment-transactions-ascribing-blame)
    - [3.2 創建撤銷合約的通道 | Creating a Channel with Contract Revocation](#32-%E5%89%B5%E5%BB%BA%E6%92%A4%E9%8A%B7%E5%90%88%E7%B4%84%E7%9A%84%E9%80%9A%E9%81%93--creating-a-channel-with-contract-revocation)
    - [3.3 nSequence 成熟度 | Sequence Number Maturity](#33-nsequence-%E6%88%90%E7%86%9F%E5%BA%A6--sequence-number-maturity)
      - [3.3.1 Timestop](#331-timestop)
      - [3.3.2 撤銷承諾交易 | Revocable Commitment Transactions](#332-%E6%92%A4%E9%8A%B7%E6%89%BF%E8%AB%BE%E4%BA%A4%E6%98%93--revocable-commitment-transactions)
      - [3.3.3 從通道兌換資金：合作交易方 | Redeeming Funds from the Channel: Cooperative Counterparties](#333-%E5%BE%9E%E9%80%9A%E9%81%93%E5%85%8C%E6%8F%9B%E8%B3%87%E9%87%91%E5%90%88%E4%BD%9C%E4%BA%A4%E6%98%93%E6%96%B9--redeeming-funds-from-the-channel-cooperative-counterparties)
      - [3.3.4 創建一個新的交易承諾，並撤銷先前的承諾 | Creating a new Commitment Transaction and Revoking Prior Commitments](#334-%E5%89%B5%E5%BB%BA%E4%B8%80%E5%80%8B%E6%96%B0%E7%9A%84%E4%BA%A4%E6%98%93%E6%89%BF%E8%AB%BE%E4%B8%A6%E6%92%A4%E9%8A%B7%E5%85%88%E5%89%8D%E7%9A%84%E6%89%BF%E8%AB%BE--creating-a-new-commitment-transaction-and-revoking-prior-commitments)
      - [3.3.5 創建可撤銷承諾交易流程 | Process for Creating Revocable Commitment Transactions](#335-%E5%89%B5%E5%BB%BA%E5%8F%AF%E6%92%A4%E9%8A%B7%E6%89%BF%E8%AB%BE%E4%BA%A4%E6%98%93%E6%B5%81%E7%A8%8B--process-for-creating-revocable-commitment-transactions)
    - [3.4 協同關閉通道 | Cooperatively Closing Out a Channel](#34-%E5%8D%94%E5%90%8C%E9%97%9C%E9%96%89%E9%80%9A%E9%81%93--cooperatively-closing-out-a-channel)
    - [3.5 雙向通道的啟示與總結 | Bidirectional Channel Implications and Summary](#35-%E9%9B%99%E5%90%91%E9%80%9A%E9%81%93%E7%9A%84%E5%95%9F%E7%A4%BA%E8%88%87%E7%B8%BD%E7%B5%90--bidirectional-channel-implications-and-summary)
  - [4 雜湊 Timelock 合約（HTLC）| Hashed Timelock Contract (HTLC)](#4-%E9%9B%9C%E6%B9%8A-timelock-%E5%90%88%E7%B4%84htlc-hashed-timelock-contract-htlc)
    - [4.1 不可撤銷的 HTLC 結構 | Non-revocable HTLC Construction](#41-%E4%B8%8D%E5%8F%AF%E6%92%A4%E9%8A%B7%E7%9A%84-htlc-%E7%B5%90%E6%A7%8B--non-revocable-htlc-construction)
    - [4.2	Off-chain 可撤銷 HTLC | Off-chain Revocable HTLC](#42-off-chain-%E5%8F%AF%E6%92%A4%E9%8A%B7-htlc--off-chain-revocable-htlc)
      - [4.2.1 當寄件者廣播的承諾交易 HTLC | HTLC when the Sender Broadcasts the Commitment Transaction](#421-%E7%95%B6%E5%AF%84%E4%BB%B6%E8%80%85%E5%BB%A3%E6%92%AD%E7%9A%84%E6%89%BF%E8%AB%BE%E4%BA%A4%E6%98%93-htlc--htlc-when-the-sender-broadcasts-the-commitment-transaction)
      - [4.2.2 接收者廣播承諾交易時的 HTLC | HTLC when the Receiver Broadcasts the Commitment Transaction](#422-%E6%8E%A5%E6%94%B6%E8%80%85%E5%BB%A3%E6%92%AD%E6%89%BF%E8%AB%BE%E4%BA%A4%E6%98%93%E6%99%82%E7%9A%84-htlc--htlc-when-the-receiver-broadcasts-the-commitment-transaction)
    - [4.3	HTLC Off-chain 終止 | 4.3	HTLC Off-chain Termination](#43-htlc-off-chain-%E7%B5%82%E6%AD%A2--43-htlc-off-chain-termination)
    - [4.4 HTLC 形成和封閉令 | HTLC Formation and Closing Order](#44-htlc-%E5%BD%A2%E6%88%90%E5%92%8C%E5%B0%81%E9%96%89%E4%BB%A4--htlc-formation-and-closing-order)
  - [5 金鑰儲存 | Key Storage](#5-%E9%87%91%E9%91%B0%E5%84%B2%E5%AD%98--key-storage)
  - [6 雙向通道的區塊鏈交易費 | Blockchain Transaction Fees for Bidirectional Channels](#6-%E9%9B%99%E5%90%91%E9%80%9A%E9%81%93%E7%9A%84%E5%8D%80%E5%A1%8A%E9%8F%88%E4%BA%A4%E6%98%93%E8%B2%BB--blockchain-transaction-fees-for-bidirectional-channels)
  - [7 薪酬合約 | Pay to Contract](#7-%E8%96%AA%E9%85%AC%E5%90%88%E7%B4%84--pay-to-contract)
  - [8 比特幣閃電網路 | The Bitcoin Lightning Network](#8-%E6%AF%94%E7%89%B9%E5%B9%A3%E9%96%83%E9%9B%BB%E7%B6%B2%E8%B7%AF--the-bitcoin-lightning-network)
    - [8.1 遞減的 Timelocks](#81-%E9%81%9E%E6%B8%9B%E7%9A%84-timelocks)
    - [8.2 付款金額 | Payment Amount](#82-%E4%BB%98%E6%AC%BE%E9%87%91%E9%A1%8D--payment-amount)
    - [8.3 清除故障和重新路由 | Clearing Failure and Rerouting](#83-%E6%B8%85%E9%99%A4%E6%95%85%E9%9A%9C%E5%92%8C%E9%87%8D%E6%96%B0%E8%B7%AF%E7%94%B1--clearing-failure-and-rerouting)
    - [8.4 付款路由 | Payment Routing](#84-%E4%BB%98%E6%AC%BE%E8%B7%AF%E7%94%B1--payment-routing)
    - [8.5 手續費 | Fees](#85-%E6%89%8B%E7%BA%8C%E8%B2%BB--fees)
  - [9 風險 | Risks](#9-%E9%A2%A8%E9%9A%AA--risks)
    - [9.1 不當 Timelocks | Improper Timelocks](#91-%E4%B8%8D%E7%95%B6-timelocks--improper-timelocks)
    - [9.2 被迫滿期的垃圾郵件 | Forced Expiration Spam](#92-%E8%A2%AB%E8%BF%AB%E6%BB%BF%E6%9C%9F%E7%9A%84%E5%9E%83%E5%9C%BE%E9%83%B5%E4%BB%B6--forced-expiration-spam)
    - [9.3 通過分裂盜竊資金 | Coin Theft via Cracking](#93-%E9%80%9A%E9%81%8E%E5%88%86%E8%A3%82%E7%9B%9C%E7%AB%8A%E8%B3%87%E9%87%91--coin-theft-via-cracking)
    - [9.4 資料丟失 | Data Loss](#94-%E8%B3%87%E6%96%99%E4%B8%9F%E5%A4%B1--data-loss)
    - [9.5 忘記及時廣播交易 | Forgetting to Broadcast the Transaction in Time](#95-%E5%BF%98%E8%A8%98%E5%8F%8A%E6%99%82%E5%BB%A3%E6%92%AD%E4%BA%A4%E6%98%93--forgetting-to-broadcast-the-transaction-in-time)
    - [9.6 無法做出必要的 Soft-Forks | Inability to Make Necessary Soft-Forks](#96-%E7%84%A1%E6%B3%95%E5%81%9A%E5%87%BA%E5%BF%85%E8%A6%81%E7%9A%84-soft-forks--inability-to-make-necessary-soft-forks)
    - [9.7 勾結礦工攻擊 | Colluding Miner Attacks](#97-%E5%8B%BE%E7%B5%90%E7%A4%A6%E5%B7%A5%E6%94%BB%E6%93%8A--colluding-miner-attacks)
  - [10 區塊大小增加與共識 | Block Size Increases and Consensus](#10-%E5%8D%80%E5%A1%8A%E5%A4%A7%E5%B0%8F%E5%A2%9E%E5%8A%A0%E8%88%87%E5%85%B1%E8%AD%98--block-size-increases-and-consensus)
  - [11 用例 | Use case](#11-%E7%94%A8%E4%BE%8B--use-case)
  - [12 結論 | Conclusion](#12-%E7%B5%90%E8%AB%96--conclusion)
  - [13 致謝 | Acknowledgements](#13-%E8%87%B4%E8%AC%9D--acknowledgements)
  - [附錄 A 解決延展性 | Resolving Malleability](#%E9%99%84%E9%8C%84-a-%E8%A7%A3%E6%B1%BA%E5%BB%B6%E5%B1%95%E6%80%A7--resolving-malleability)
  - [參考文獻 | References](#%E5%8F%83%E8%80%83%E6%96%87%E7%8D%BB--references)

## 摘要 | Abstract

The bitcoin protocol can encompass the global financial transaction volume in all electronic payment systems today, without a single custodial third party holding funds or requiring participants to have anything more than a computer using a broadband connection. A decentralized system is proposed whereby transactions are sent over a network of micropayment channels (a.k.a. payment channels or transaction channels) whose transfer of value occurs off-blockchain. If Bitcoin transactions can be signed with a new sighash type that addresses malleability, these transfers may occur between untrusted parties along the transfer route by contracts which, in the event of uncooperative or hostile participants, are enforceable via broadcast over the bitcoin blockchain in the event of uncooperative or hostile participants, through a series of decrementing timelocks.

如今比特幣協議可以涵蓋全球金融所有的電子支付系統的交易量，沒有單一的一個協力廠商保管或持有資金，或要求參加者除了有使用寬頻連線的電腦之外其他的什麼東西。分散式系統表明交易是被發送到一個小額支付的通道網路（又名支付通道和交易通道），其價值轉移發生在 off-blockchain 的情況下。如果比特幣的交易可以在一種新的強調延展性的類型條件下簽署，這些轉移可以在不受信任的雙方之間通過合約沿著傳送路徑進行，在一系列遞減時間鎖鏈中，如果有非合作或敵對的參與者，則採取在比特幣區塊鏈上強制執行的辦法。

---

## 1 比特幣區塊鏈可擴展性問題 | The Bitcoin Blockchain Scalability Problem

The Bitcoin[1] blockchain holds great promise for distributed ledgers, but the blockchain as a payment platform, by itself, cannot cover the world’s commerce anytime in the near future. The blockchain is a gossip protocol whereby all state modifications to the ledger are broadcast to all participants. It is through this “gossip protocol” that consensus of the state, everyone’s balances, is agreed upon. If each node in the bitcoin network must know about every single transaction that occurs globally, that may create a significant drag on the ability of the network to encompass all global financial transactions. It would instead be desirable to encompass all transactions in a way that doesn’t sacrifice the decentralization and security that the network provides.

比特幣[1]區塊鏈在擁有分散式分類帳方面很有前景，但在不久將來的某個時間，會出現區塊鏈作為一個支付平臺，其本身不能覆蓋全球的電子商務的情況。區塊鏈是一個八卦協議，把所有國家向總帳發的更改發佈給所有的參與者。國家的共識，每個人的餘額通過這種“八卦協定”達成一致。如果在比特幣網路中的每個節點必須瞭解在全球範圍發生的每一個交易，可能造成阻礙網路涵蓋全球所有金融交易的能力。相反，若能涵蓋全球所有金融交易，並且不會使分散化和安全性受到損害，這才是我們需要的。

---

The payment network Visa achieved 47,000 peak transactions per second (tps) on its network during the 2013 holidays[2], and currently averages hundreds of millions per day. Currently, Bitcoin supports less than 7 transactions per second with a 1 megabyte block limit. If we use an average of 300 bytes per bitcoin transaction and assumed unlimited block sizes, an equivalent capacity to peak Visa transaction volume of 47,000/tps would be nearly 8 gigabytes per Bitcoin block, every ten minutes on average. Continuously, that would be over 400 terabytes of data per year.

支付網路 Visa 在 2013 假期期間[2]，在其網路上每秒實現 47000 交易（TPS），目前實現平
均每天數億筆交易。目前，比特幣因為 1 MB塊的限制，每秒僅支持小於 7 筆交易。如果 每次比特幣交易我們平均用 300 Byte，並假設塊大小無限制，達到與 Visa 峰值 47000 / TPS 的交易量同等資料容量意味著每十分鐘每比特幣區塊將近 8 GB資料。持續下去，每年的資料將超過 400 TB。

---

Clearly, achieving Visa-like capacity on the Bitcoin network isn’t feasible today. No home computer in the world can operate with that kind of bandwidth and storage. If Bitcoin is to replace all electronic payments in the future, and not just Visa, it would result in outright collapse of the Bitcoin network, or at best, extreme centralization of Bitcoin nodes and miners to the only ones who could afford it. This centralization would then defeat aspects of network decentralization that make Bitcoin secure, as the ability for entities to validate the chain is what allows Bitcoin to ensure ledger accuracy and security.

顯然，如今在比特幣網路上獲得 Visa 般的能力是不可行的。在世界上沒有家用電腦可以有那樣的頻寬和儲存。如果比特幣在未來替換所有的電子支付，而不僅僅是 Visa，這將導致比特幣網路的徹底崩潰，或者在最好的情況下，只有可以支付得起的比特幣節點和礦工可以使用。這種集中化會再次打敗網路分散化，使比特幣安全成為具有確保總帳的準確性和安全性 的能力的實體。

---

Having fewer validators due to larger blocks not only implies fewer individuals ensuring ledger accuracy, but also results in fewer entities that would be able to validate the blockchain as part of the mining process, which results in encouraging miner centralization. Extremely large blocks, for example in the above case of 8 gigabytes every 10 minutes on average, would imply that only a few parties would be able to do block validation. This creates a great possibility that entities will end up trusting centralized parties. Having privileged, trusted parties creates a social trap whereby the central party will not act in the interest of an individual (principalagent problem), e.g. rentierism by charging higher fees to mitigate the incentive to act dishonestly. In extreme cases, this manifests as individuals sending funds to centralized trusted custodians who have full custody of customers’ funds. Such arrangements, as are common today, create severe counterparty risk. A prerequisite to prevent that kind of centralization from occurring would require the ability for bitcoin to be validated by a single consumer-level computer on a home broadband connection. By ensuring that full validation can occur cheaply, Bitcoin nodes and miners will be able to prevent extreme centralization and trust, which ensures extremely low transaction fees.

由於較大區塊而只有更少的驗證器不僅意味著更少數量的個人來確保總帳精度，也導致在開采過程中較少的實體能夠驗證區塊鏈，這將鼓勵礦工集中化。非常大的區塊，例如在上述情況下平均每 10 分鐘 8 GB，將意味著只有少數能夠驗證區塊。這就產生了一個實體會相信集中方的可能性。有特權的，值得信賴的集中方創建一個社交陷阱，由此集中方不 會在以個人（委託-代理問題）的利益為主，如承租人通過收取較高的手續費，以減輕行事不誠實的傾向。在極端的情況下，這表現為個人給擁有客戶資金的充分的監管權的集中方發送資金。這樣的安排，如今是非常常見的，產生嚴重的交易對方風險。防止那種集權發生的一個先決條件需要比特幣有這樣一種能力，通過在家用寬頻連線的單一電腦進行驗證。通過 確保以較低的資金獲得充分的驗證，比特幣節點和礦工將能夠避免極端的集權和信任，確保極低的交易手續費。

---

While it is possible that Moore’s Law will continue indefinitely, and the computational capacity for nodes to cost-effectively compute multigigabyte blocks may exist in the future, it is not a certainty.

摩爾定律無限期地繼續是有可能的，並且在未來，能使節點具有以低成本高效益的計算多GB的區塊的計算能力，但是那不是確定的。

---

To achieve much higher than 47,000 transactions per second using Bitcoin requires conducting transactions off the Bitcoin blockchain itself. It would be even better if the bitcoin network supported a near-unlimited number of transactions per second with extremely low fees for micropayments. Many micropayments can be sent sequentially between two parties to enable any size of payments. Micropayments would enable unbunding, less trust and commodification of services, such as payments for per-megabyte internet service. To be able to achieve these micropayment use cases, however, would require severely reducing the amount of transactions that end up being broadcast on the global Bitcoin blockchain.

為了實現用比特幣進行每秒多於 47000 筆交易，需要脫離比特幣區塊鏈本身進行交易。 如果比特幣網路支援以極低的手續費每秒進行近乎無限數量的小額交易會更好。許多小額支付 可以按順序在兩方之間發送，使任何大小的付款成為可能。小額支付將使服務變得非捆束， 少信任，商品化。如支付每MB的互聯網服務。為了能夠實現這些小額用例，將需要嚴重 降低最終被廣播的全球比特幣區塊鏈交易的數量。

---

While it is possible to scale at a small level, it is absolutely not possible to handle a large amount of micropayments on the network or to encompass all global transactions. For bitcoin to succeed, it requires confidence that if it were to become extremely popular, its current advantages stemming from decentralization will continue to exist. In order for people today to believe that Bitcoin will work tomorrow, Bitcoin needs to resolve the issue of block size centralization effects; large blocks implicitly create trusted custodians and significantly higher fees.

雖然可以在一個小規模水準上進行，在網路上處理大量小額支付或者包含全球交易是絕對不可能的。比特幣若想成功，它需要這樣一種信心，如果它能變得非常流行，其目前的由權力下放所產生的優勢將繼續存在。為了讓今天的人們相信比特幣在將來能有用，比特幣需要解決區塊大小集中效果；大區塊自主創建值得信賴的保管人和高手續費的問題。

---

## 2 小額支付通道可以解決可擴展性問題 | A	Network	of	Micropayment	Channels	Can Solve Scalability
> “If a tree falls in the forest and no one is around to hear it, does it make a sound?”
> 
> “如果一棵樹倒在森林裡，周圍沒有人聽到它，它會發出聲音嗎？”

---

The above quote questions the relevance of unobserved events —if nobody hears the tree fall, whether it made a sound or not is of no consequence. Similarly, in the blockchain, if only two participants care about an everyday recurring transaction, it’s not necessary for all other nodes in the bitcoin network to know about that transaction. It is instead preferable to only have the bare minimum of information on the blockchain. By deferring telling the entire world about every transaction, doing net settlement of their relationship at a later date enables Bitcoin users to conduct many transactions without bloating up the blockchain or creating trust in a centralized counterparty. An effectively trustless structure can be achieved by using time locks as a component to global consensus.

上面的引文質疑未觀察事件的相關性 - 如果沒有人聽到樹倒下，它是否發出聲音是無關緊要的。 類似地，在區塊鏈中，如果只有兩個參與者關心日常重複交易，則比特幣網絡中的所有其他節點不必知道該交易。 相反，最好只有區塊鏈上最少的信息。 通過推遲向全世界講述每筆交易，在以後對其關係進行淨結算使得比特幣用戶能夠進行許多交易，而不會使區塊鏈膨脹或在集中交易對方中建立信任。 通過使用時間鎖作為全球共識的組成部分，可以實現有效無信任的結構。

---

Currently the solution to micropayments and scalability is to offload the transactions to a custodian, whereby one is trusting third party custodians to hold one’s coins and to update balances with other parties. Trusting third parties to hold all of one’s funds creates counterparty risk and transaction costs.

目前的小額支付和可擴展性解決方案將交易轉交給一個託管人，由一個被信任的協力廠商託管來持有硬幣並更新與其他各方的餘額情況。信任協力廠商來保存所有的人的資金可能產生交易對方風險和交易成本。

---

Instead, using a network of these micropayment channels, Bitcoin can scale to billions of transactions per day with the computational power available on a modern desktop computer today. Sending many payments inside a given micropayment channel enables one to send large amounts of funds to another party in a decentralized manner. These channels are not a separate trusted network on top of bitcoin. They are real bitcoin transactions.

相反，使用這些小額支付通道的網路，比特幣可以擴展到當今在現代筆記型電腦上以強大的計算能力進行數十億美元的交易。在一個小額支付通道中進行大量支付使人們能夠以分散的方式發送大量的資金給另一方。這些通道在比特幣上不是一個單獨的可信網路。他們是真正的比特幣交易。

---

Micropayment channels[3][4] create a relationship between two parties to perpetually update balances, deferring what is broadcast to the blockchain in a single transaction netting out the total balance between those two parties. This permits the financial relationships between two parties to be trustlessly deferred to a later date, without risk of counterparty default. Micropayment channels use real bitcoin transactions, only electing to defer the broadcast to the blockchain in such a way that both parties can guarantee their current balance on the blockchain; this is not a trusted overlay network —payments in micropayment channels are real bitcoin communicated and exchanged off-chain.

小額支付通道[3][4]在雙方之間建立起關係，來更新餘額，決定在雙方交易時產生的總餘額中被推遲廣播到區塊鏈的部分。這使得雙方之間的財務關係被不可信地推遲到以後的日子，沒有交易對方違約的風險。小額支付通道使用真實的比特幣交易，只有通過選舉的方式來決定推遲在區塊鏈中廣播的部分，雙方才可以保證其在區塊鏈上現有的餘額;這不是值得信賴的覆蓋網路-在小額支付通道發生的支付是真正比特幣 off-chain 的溝通與交換。

---

### 2.1 小額支付通道不要求信託 | Micropayment Channels Do Not Require Trust

Like the age-old question of whether the tree falling in the woods makes a sound, if all parties agree that the tree fell at 2:45 in the afternoon, then the tree really did fall at 2:45 in the afternoon. Similarly, if both counterparties agree that the current balance inside a channel is 0.07 BTC to Alice and 0.03 BTC to Bob, then that’s the true balance. However, without cryptography, an interesting problem is created: If one’s counterparty disagrees about the current balance of funds (or time the tree fell), then it is one’s word against another. Without cryptographic signatures, the blockchain will not know who owns what.

就像樹倒在樹林裡是否發出聲音的老問題，如各方均同意該樹在 2:45 倒下，那麼該樹確實在下午 2:45 倒下。同樣，如果雙方均同意，通道內現有的餘額為 0.07 BTC 給 Alice 和 0.03 BTC 給 Bob，那麼這就是真正的餘額。然而，如果沒有密碼，一個有趣的問題產生了：如果其中一方不同意有關資金的當前餘額（樹倒下的時間），那麼雙方就產生了分歧。如果沒有加密 的簽名，區塊鏈就不知道誰擁有什麼。

---

If the balance in the channel is 0.05 BTC to Alice and 0.05 BTC to Bob, and the balance after a transaction is 0.07 BTC to Alice and 0.03 BTC to Bob, the network needs to know which set of balances is correct. Blockchain transactions solve this problem by using the blockchain ledger as a timestamping system. At the same time, it is desirable to create a system which does not actively use this timestamping system unless absolutely necessary, as it can become costly to the network.

如果在通道中的餘額為 0.05 BTC 給 Alice 和 0.05 BTC 給 Bob，一個交易後的餘額為 0.07 BTC 給 Alice 和 0.03 BTC 給 Bob，網路需要知道哪個餘額集是正確的。區塊鏈交易通過使用區塊鏈總帳作為時間系統解決了這個問題。與此同時，希望建立一個系統，該系統除必要情況不積極地使用該時間戳記系統，因為它對於網路來說是昂貴的。

---

Instead, both parties can commit to signing a transaction and not broadcasting this transaction. So if Alice and Bob commit funds into a 2-of-2 multisignature address (where it requires consent from both parties to create spends), they can agree on the current balance state. Alice and Bob can agree to create a refund from that 2-of-2 transaction to themselves, 0.05 BTC to each. This refund is not broadcast on the blockchain. Either party may do so, but they may elect to instead hold onto that transaction, knowing that they are able to redeem funds whenever they feel comfortable doing so. By deferring broadcast of this transaction, they may elect to change this balance at a future date.

相反，雙方可以承諾簽署一個交易，但並不廣播該交易。因此，如果 Alice 和 Bob 投入資金到 2-of-2 多重簽名地址（其要求雙方同意來產生花費），他們都同意目前的餘額狀態。Alice 和 Bob 可以要求從 2-of-2 交易中退款給自己，每人 0.05 BTC。這份退款不會被廣播到區塊鏈。任何一方都可以這樣做，但他們更可能選擇堅持進行該交易，明知自己有能力在自己希望時撤回資金。通過推遲本次交易的廣播，他們可能會選擇在未來某一日期改變這種餘額。

---

To update the balance, both parties create a new spend from the 2-of-2 multisignature address, for example 0.07 to Alice and 0.03 to Bob. Without proper design, though, there is the timestamping problem of not knowing which spend is correct: the new spend or the original refund.

要更新這種餘額，雙方產生 2-of-2 的多重簽名地址的新支出，例如 0.07 給 Alice 和 0.03 給 Bob。如果沒有適當的設計，會產生時間戳記問題，不知道哪一項花費是正確的：新的支出還是原來的退款。

---

The restriction on timestamping and dates, however, is not as complex as full ordering of all transactions as in the bitcoin blockchain. In the case of micropayment channels, only two states are required: the current correct balance, and any old deprecated balances. There would only be a single correct current balance, and possibly many old balances which are deprecated.

在時間戳記和日期上的限制，不是像在比特幣區塊鏈一樣複雜和有序。在小額通道的情況下，只有兩個狀態是必需的：當前的正確的餘額，和任何舊的棄用餘額。只會有一個正確的現有餘額，可能有很多不建議使用的舊餘額。

---

Therefore, it is possible in bitcoin to devise a bitcoin script whereby all old transactions are invalidated, and only the new transaction is valid. Invalidation is enforced by a bitcoin output script and dependent transactions which force the other party to give all their funds to the channel counterparty. By taking all funds as a penalty to give to the other, all old transactions are thereby invalidated.

因此，在比特幣中可以設計比特幣腳本，從而使所有舊交易無效，並且只有新交易有效。 失效由比特幣輸出腳本和依賴交易強制執行，這迫使另一方將所有資金提供給通道對方。 通過將所有資金作為懲罰給予對方，所有舊交易因此無效。

---

This invalidation process can exist through a process of channel consensus where if both parties agree on current ledger states (and building new states), then the real balance gets updated. The balance is reflected on the blockchain only when a single party disagrees. Conceptually, this system is not an independent overlay network; it is more a deferral of state on the current system, as the enforcement is still occurring on the blockchain itself (albeit deferred to future dates and transactions).

這種失效過程可通過通道的共識，其中，如果雙方都同意目前的分類帳狀態（和建立新的狀態）過程存在，那麼真正的餘額得到更新。僅在一個單一方不同意時才在區塊鏈上反映出來。從概念上講，這種系統不是一個獨立的覆蓋網路;它是在現行系統上的一個延遲的狀態，因為強制執行仍在區塊鏈上發生（儘管推遲到將來的日期和交易）。

---

### 2.2 通道網路 | A Network of Channels

Thus, micropayment channels only create a relationship between two parties. Requiring everyone to create channels with everyone else does not solve the scalability problem. Bitcoin scalability can be achieved using a large network of micropayment channels.

因此，小額支付通道只創立雙方之間的關係。要求大家與其他人建立通道不解決擴展性問題。比特幣的可擴展性可以通過小額支付通道的一個大的網路來實現。
 
---

If we presume a large network of channels on the Bitcoin blockchain, and all Bitcoin users are participating on this graph by having at least one channel open on the Bitcoin blockchain, it is possible to create a near-infinite amount of transactions inside this network. The only transactions that are broadcasted on the Bitcoin blockchain prematurely are with uncooperative channel counterparties.

如果我們假定一個比特幣區塊鏈通道的大型網路，並且所有參與的比特幣用戶在比特幣區塊鏈上具有至少一個開放通道，在該網路內可以創建近於無限量的交易。在比特幣區塊鏈上過早地廣播的唯一的交易是存在不合作通道對方的交易。

---

By encumbering the Bitcoin transaction outputs with a hashlock and timelock, the channel counterparty will be unable to outright steal funds and Bitcoins can be exchanged without outright counterparty theft. Further, by using staggered timeouts, it’s possible to send funds via multiple intermediaries in a network without the risk of intermediary theft of funds.

通過雜湊鏈和時間鏈延遲比特幣交易輸出，通道對方將無法直接竊取資金和比特幣可以在無對方竊取的情況下直接交換。此外，通過使用交錯休息，在沒有資金仲介竊取的風險的條件下通過多個在網路中的仲介機構發送資金成為可能。

---

## 3 雙向支付通道 | Bidirectional Payment Channels

Micropayment channels permit a simple deferral of a transaction state to be broadcast at a later time. The contracts are enforced by creating a responsibility for one party to broadcast transactions before or after certain dates. If the blockchain is a decentralized timestamping system, it is possible to use clocks as a component of decentralized consensus[5] to determine data validity, as well as present states as a method to order events[6].

小額支付通道允許交易狀態簡單推遲至稍後時間廣播。該合約是以這樣的方式執行，創造一方在一定日期之前或之後廣播交易的責任。如果區塊鏈是一個分散化的時間戳記系統，它可以使用時鐘作為分散共識[5]的組成部分，以確定資料有效性，以及展示當前狀態作為訂購事件的方法[6]。

---

By creating timeframes where certain states can be broadcast and later invalidated, it is possible to create complex contracts using bitcoin transaction scripts. There has been prior work for Hub-and-Spoke Micro-payment Channels[7][8][9].(and trusted payment channel networks[10][11]) looking at building a hub-and-spoke network today. However, Lightning Network’s bidirectional micropayment channel requires the malleability soft-fork described in Appendix A to enable near-infinite scalability while miti-gating risks of intermediate node default.

通過創建特定狀態的廣播或失效的時間表，就可以使用比特幣交易腳本創建複雜的合約。已經有前期工作的中心輻射型小額支付通道[7][8][9]（和值得信賴的支付通道網路[10][11]）監控今日建立樞紐和輻射網路的過程。然而，閃電定位網路的雙向小額通道要求在附錄 A 中所述的可塑性 Softfork，使在控制中間節點出錯風險時有近乎於無限的可擴展性。

---

By chaining together multiple micropayment channels, it is possible to create a network of transaction paths. Paths can be routed using a BGP-like system, and the sender may designate a particular path to the recipient. The output scripts are encumbered by a hash, which is generated by the recipient. By disclosing the input to that hash, the recipient’s counterparty will be able to pull funds along the route.

通過把多個微支付通道串聯起來，有可能創建交易路徑的網路。路徑可以使用類似 BGP 的系統進行路由，並且發送方可以指定一個特殊的路徑給收件人。輸出腳本由接收者產生的雜湊密碼限制。通過公開的輸入雜湊密碼，收件人的對方就能沿線拉動資金。

---

### 3.1 頻道創建中存在的問題

In order to participate in this payment network, one must create a micropayment channel with another participant on this network.

為了參加本次支付網路，我們必須與其他參與者創建這個網路上的小額支付通道。

---

#### 3.1.1 創建無簽署的資金交易 | The Problem of Blame in Channel Creation

An initial channel Funding Transaction is created whereby one or both channel counterparties fund the inputs of this transaction. Both parties create the inputs and outputs for this transaction but do not sign the transaction. 

最初的提供的資金交易的通道創建起來是由通道的一方或者雙方輸入本次交易的資金。雙方建立這項交易的輸入和輸出，但不簽署交易。

---

The output for this Funding Transaction is a single 2-of-2 multisigna-
ture script with both participants in this channel, henceforth named Alice and Bob. Both participants do not exchange signatures for the Funding Transaction until they have created spends from this 2-of-2 output refunding the original amount back to its respective funders. The purpose of not signing the transaction allows for one to spend from a transaction which does not yet exist. If Alice and Bob exchange the signatures from the Funding Transaction without being able to broadcast spends from the Funding Transaction, the funds may be locked up forever if Alice and Bob do not cooperate (or other coin loss may occur through hostage scenarios whereby one pays for the cooperation from the counterparty).

對於這筆資金交易的輸出是參加這個通道雙方的 2-of-2 的多重簽名，今後命名為 Alice 和 Bob 腳本。這兩個參與者沒有為資金交易交換簽名，直到他們已經從 2-of-2 得到了與原來金額相等的退還金額。未簽署交易的目的，是允許從一個尚不存在一個交易中花費。如果 Alice 和 Bob 在資金交易中交換了簽名，而不能得到資金交易的廣播，而且如果 Alice 和 Bob 不配合，資金可能會被永久的鎖定（或由一方承擔不合作的損失）。

---

Alice and Bob both exchange inputs to fund the Funding Transaction (to know which inputs are used to determine the total value of the channel), and exchange one key to use to sign with later. This key is used for the 2-of-2 output for the Funding Transaction; both signatures are needed to spend from the Funding Transaction, in other words, both Alice and Bob need to agree to spend from the Funding Transaction.

Alice 和 Bob 雙方交換輸入來提供資金交易所需資金（知道哪些輸入用於確定通道的總價值），來交換之後用來簽署的鑰匙。此鑰匙用於資金交易的 2-of-2 輸出;資金花費的產生需要雙方的簽署，換句話說，Alice 和 Bob 必須同意從資金交易中的資金花費。

---

#### 3.1.2 來自未簽署交易的消費 | Spending from an Unsigned Transaction

The Lightning Network uses a SIGHASH NOINPUT transaction to spend from this 2-of-2 Funding Transaction output, as it is necessary to spend from a transaction for which the signatures are not yet exchanged. SIGHASH NOINPUT, implemented using a soft-fork, ensures transactions can be spent from before it is signed by all parties, as transactions would need to be signed to get a transaction ID without new sighash flags. Without SIGHASH NOINPUT, Bitcoin transactions cannot be spent from before they may be broadcast —it’s as if one could not draft a contract without paying the other party first. SIGHASH NOINPUT resolves this problem. See Appendix A for more information and implementation.

閃電網路使用的是 SIGHASH NOINPUT 交易，從 2-of-2 輸出資金交易花費，因為這對於從尚未交換其簽名的交易上花費是必須的。SIGHASH NOINPUT，用 Softfork 實施，確保交易能夠在各方簽署之前執行，因為交易需要登錄才能獲取沒有新的 sighash flags 交易。如果沒有 SIGHASH NOINPUT，比特幣交易無法在廣播之前進行-就好像一個人不能在沒有支付對方的前提下得到草本。 SIGHASH NOINPUT 解決了這一問題。更多的資訊和實施見附錄 A。

---

Without SIGHASH NOINPUT, it is not possible to generate a spend from a transaction without exchanging signatures, since spending the Funding Transaction requires a transaction ID as part of the signature in the child’s input. A component of the Transaction ID is the parent’s (Funding Transaction’s) signature, so both parties need to exchange their signatures of the parent transaction before the child can be spent. Since one or both parties must know the parent’s signatures to spend from it, that means one or both parties are able to broadcast the parent (Funding Transaction) before the child even exists. SIGHASH NOINPUT gets around this by permitting the child to spend without signing the input. With SIGHASH NOINPUT, the order of operations are to:
1. Create the parent (Funding Transaction)
2. Create the children (Commitment Transactions and all spends from the commitment transactions)
3. Sign the children
4. Exchange the signatures for the children
5. Sign the parent
6. Exchange the signatures for the parent
7. Broadcast the parent on the blockchain


如果沒有 SIGHASH NOINPUT，不可能產生在不交換簽名的情況下進行交易支出，因為花費的資金交易需要一個交易 ID 作為子輸入簽名的一部分。交易 ID 的一個組成部分是父（交易資金的來源）的簽名，因此雙方需要交換自己的父簽名子輸入才可以花費。由於一方或雙方必須知道父簽名因而由它來消費，這意味著一方或雙方都能夠在子輸入存在之前廣播父簽名（融資交易）。SIGHASH NOINPUT 通過允許子輸入無需登錄輸入就可消費來解決這個問題。SIGHASH NOINPUT 的操作順序是：

1. 創建父輸入（融資交易）
2. 創建子輸入（承諾交易及從承諾交易的所有花費）
3. 登錄子輸入
4. 交換子簽名
5. 簽署父簽名
6. 交換父簽名
7. 廣播區塊鏈上的父簽名

---

One is not able to broadcast the parent (Step 7) until Step 6 is complete. Both parties have not given their signature to spend from the Funding Transaction until step 6. Further, if one party fails during Step 6, the parent can either be spent to become the parent transaction or the inputs to the parent transaction can be double-spent (so that this entire transaction path is invalidated).

一方不能夠廣播父簽名（步驟 7），直到步驟 6 完成。雙方直到步驟 6 才交換他們資金交易
的簽名。此外，如果步驟 6 中一方出現故障，父輸入可以成為父交易或者父交易會產生雙花（這樣，整個交易路徑無效）。

---

#### 3.1.3 承諾交易：不可執行的結構 | Commitment Transactions: Unenforcible Construction

After the unsigned (and unbroadcasted) Funding Transaction has been created, both parties sign and exchange an initial Commitment Transaction. These Commitment Transactions spends from the 2-of-2 output of the Funding Transaction (parent). However, only the Funding Transaction is broadcast on the blockchain.

無簽署（和無廣播）資金交易創建後，雙方簽署並交換了最初的承諾交易。這些承諾交易花 費來自於 2-of-2 的資金交易（父）輸出。但是，只有資金交易在區塊鏈上廣播。

---

Since the Funding Transaction has already entered into the blockchain, and the output is a 2-of-2 multisignature transaction which requires the agreement of both parties to spend from, Commitment Transactions are used to express the present balance. If only one 2-of-2 signed Commitment Transaction is exchanged between both parties, then both parties will be sure that they are able to get their money back after the Funding Transaction enters the blockchain. Both parties do not broadcast the Commitment Transactions onto the blockchain until they want to close out the current balance in the channel. They do so by broadcasting the present Commitment Transaction.

由於資金交易已經進入區塊鏈，輸出為需要雙方的協定的 2-of-2 的多重簽名交易，承諾交易是用來表達目前的餘額。只有一個 2-of-2 簽字承諾交易在雙方之間進行交換，那麼雙方將確保他們能拿回自己投入區塊鏈資金交易的錢。雙方不在區塊鏈廣播承諾交易到直到他們想從通道中停止現有的餘額。他們通過廣播現有的承諾交易來達到此目的。

---

Commitment Transactions pay out the respective current balances to each party. A naive (broken) implementation would construct an unbroadcasted transaction whereby there is a 2-of-2 spend from a single transaction which have two outputs that return all current balances to both channel counterparties. This will return all funds to the original party when creating an initial Commitment Transaction.

承諾交易支付當前餘額的相應每一方。一個單純（破碎）的實施將構建一個不廣播交易，借此有從單一的交易方到交易對方的 2-of-2 的支出，這個單一的交易方具有兩個返回當前餘額的輸出。這將創建一個初始的承諾交易，返回原方所有的資金。

---

![](image/figure1.png)

Figure 1: A naive broken funding transaction is described in this diagram. The Funding Transaction (F), designated in green, is broadcast on the blockchain after all other transactions are signed. All other transactions spending from the funding transactions are not yet broadcast, in case the counterparties wish to update their balance. Only the Funding Transaction is broadcast on the blockchain at this time.

圖 1：一個真正破碎的資金交易將在本圖中描述。資金交易（F），被標記為綠色，在所有其他交易簽署後被廣播被區塊鏈上。從資金交易支出的所有其他交易都還沒有廣播，以防對方想要更新自己的餘額。只有在這個時候，資金交易才能廣播在區塊鏈上。

---

For instance, if Alice and Bob agree to create a Funding Transaction with a single 2-of-2 output worth 1.0 BTC (with 0.5 BTC contribution from each), they create a Commitment Transaction where there are two 0.5 BTC outputs for Alice and Bob. The Commitment Transactions are signed first and keys are exchanged so either is able to broadcast the Commitment Transaction at any time contingent upon the Funding Transaction entering into the blockchain. At this point, the Funding Transaction signatures can safely be exchanged, as either party is able to redeem their funds by broadcasting the Commitment Transaction.

例如，如果 Alice 和 Bob 同意用 2-of-2 輸出來創建一個價值 1.0 BTC（各自貢獻 0.5 BTC）的資金交易，他們創造一個有分別來自 Alice 和 Bob 兩個輸出的承諾交易。該承諾首先應被簽署，並且交換金鑰，因此交易雙方能在與資金交易進入區塊鏈的任意合適的時間來公布承諾交易。在這一點上，資金交易簽名可以安全地進行交換，因為任何一方能夠通過廣播的承諾交易贖回自己的資金。

---

This construction breaks, however, when one wishes to update the present balance. In order to update the balance, they must update their Commitment Transaction output values (the Funding Transaction has already entered into the blockchain and cannot be changed).

但是，這種結構在當一個人希望更新餘額時會斷裂。為了更新餘額，就必須更新自己的承諾交易的輸出值（融資交易已經進入區塊鏈，不能更改）。

---

When both parties agree to a new Commitment Transaction and exchange signatures for the new Commitment Transaction, either Commitment Transactions can be broadcast. As the output from the Funding Transaction can only be redeemed once, only one of those transactions will be valid. For instance, if Alice and Bob agree that the balance of the channel is now 0.4 to Alice and 0.6 to Bob, and a new Commitment Transaction is created to reflect that, either Commitment Transaction can be broadcast. In effect, one would be unable to restrict which Commitment Transaction is broadcast, since both parties have signed and exchanged the signatures for either balance to be broadcast.

當雙方都同意一個新的承諾交易並且為了新承諾交易交換簽名，任意承諾交易可以被廣播。輸出從資金交易中只能被贖回一次，這些交易中只有一個將是有效的。例如，如果 Alice 和 Bob 同意通道的餘額為 0.4 給 Alice 和 0.6 給 Bob，一個新的交易承諾將被重新創建並且任何承諾交易可以被廣播。事實上，為了被廣播的任何餘額，雙方都已經簽署了並交換了簽名，一方將無法限制其承諾交易是否廣播。

---

![](image/figure2.png)

Figure 2: Either of the Commitment Transactions can be broadcast any any time by either party, only one will successfully spend from the single Funding Transaction. This cannot work because one party will not want to broadcast the most recent transaction.

圖 2：承諾交易可由任何一方在任何時間廣播，只有一方會從單一的資金交易中成功花費。這樣不行，因為一方不想廣播最新的交易。

---

Since either party may broadcast the Commitment Transaction at any time, the result would be after the new Commitment Transaction is generated, the one who receives less funds has significant incentive to broadcast the transaction which has greater values for themselves in the Commitment Transaction outputs. As a result, the channel would be immediately closed and funds stolen. Therefore, one cannot create payment channels under this model.

因為任何一方都可以在任何時間廣播承諾交易，其結果是，產生了新的承諾後，獲得資金少的人想廣播這承諾交易，在這承諾交易產出中對他們具有更大的價值。其結果是，該通道將被立即關閉並且資金被盜。因此，人們不能在這種模式下建立支付通道。

---

#### 3.1.4 承諾交易：指出禍源 | Commitment Transactions: Ascribing Blame

Since any signed Commitment Transaction may be broadcast on the blockchain, and only one can be successfully broadcast, it is necessary to prevent old Commitment Transactions from being broadcast. It is not possible to revoke tens of thousands of transactions in Bitcoin, so an alternate method is necessary. Instead of active revocation enforced by the blockchain, it’s necessary to construct the channel itself in similar manner to a Fidelity Bond, whereby both parties make commitments, and violations of these commitments are enforced by penalties. If one party violates their agreement, then they will lose all the money in the channel.

因為任何簽署的承諾交易可以被廣播在區塊鏈上，並且只有一個可以成功地廣播，有必要防止舊承諾交易被廣播。撤銷在比特幣上的幾萬交易是不可能的，所以另一種方法是必要的。相反，主動撤銷區塊鏈強制執行的交易，有必要以與富達債券類似的方式建立通道，即雙方都作出承諾，違反這些承諾被強制實施處罰。如果一方違反了他們的協議，那麼他們將失去所有在通道中的錢。

---

For this payment channel, the contract terms are that both parties commit to broadcasting only the most recent transaction. Any broadcast of older transactions will cause a violation of the contract, and all funds are given to the other party as a penalty.

對於這種支付通道，合約條款是：雙方承諾只廣播最近的交易，對舊交易的任何廣播將導致對合約的違反，所有的資金都作為懲罰送給對方。

---

This can only be enforced if one is able to ascribe blame for broadcasting an old transaction. In order to do so, one must be able to uniquely identify who broadcast an older transaction. This can be done if each counterparty has a uniquely identifiable Commitment Transaction. Both parties must sign the inputs to the Commitment Transaction which the other party is responsible for broadcasting. Since one has a version of the Commitment Transaction that is signed by the other party, one can only broadcast one’s own version of the Commitment Transaction.

這只有在一方能夠將責任歸咎於廣播舊交易的情況下才可以被執行。為了做到這一點，必須能夠準確的識別是誰廣播了一個舊交易。這是可以做到，如果雙方擁有唯一地可鑒定的承諾交易。雙方必須簽署承諾交易，而對方負責廣播。因為一方由對方簽署的一份交易承諾，一方只能廣播自己承諾交易的版本。

---

For the Lightning Network, all spends from the Funding Transaction output, Commitment Transactions, have two half-signed transactions. One Commitment Transaction in which Alice signs and gives to Bob (C1b), and another which Bob signs and gives to Alice (C1a). These two Commitment Transactions spend from the same output (Funding Transaction), and have different contents; only one can be broadcast on the blockchain, as both pairs of Commitment Transactions spend from the same Funding Transaction. Either party may broadcast their received Commitment Transaction by signing their version and including the counterparty’s signature. For example, Bob can broadcast Commitment C1b, since he has already received the signature for C1b from Alice —he includes Alice’s signature and signs C1b himself. The transaction will be a valid spend from the Funding Transaction’s 2-of-2 output requiring both Alice and Bob’s signature.

對於閃電網，一切花費從資金交易輸出，承諾交易有兩個半簽名交易。Alice 簽署一份承諾交易，給 Bob（C1b），另一半由 Bob 簽署，給 Alice（C1a）。這兩個承諾的花費來自於相同的輸出（融資交易），並有不同的內容;只有一個可以在區塊鏈廣播，因為交易承諾的兩部分來自同一交易的資金支出。任何一方都可以通過登錄包括交易對方簽名自己的版本，廣播其收到的承諾交易。例如，Bob 可以廣播承諾 C1b，因為他已經從 Alice 那裡收到了 C1b 的簽名-它包含了 Alice 的簽名和 C1b 的自己的簽名。該交易將會從 2-of-2 資金交易的輸出要求 Alice 和 Bob 的簽名有效支出。

---

![](image/figure3.png)

Figure 3: Purple boxes are unbroadcasted transactions which only Alice can broadcast. Blue boxes are unbroadcasted transaction which only Bob can broadcast. Alice can only broadcast Commitment 1a, Bob can only broadcast Commitment 1b. Only one Commitment Transaction can be spent from the Funding Transaction output. Blame is ascribed, but either one can still be spent with no penalty.

圖 3：紫框裡代表未被廣播只有 Alice 可以廣播的交易。籃框裡代表未被廣播只有 Bob 可以 廣播的交易。Alice 只能廣播承諾 1A，Bob 只能廣播承諾 1B。只有一個承諾交易可以從交易資金輸出中支出。錯誤根源已找出，但任何一方仍然可以花費並且不受懲罰。

---

However, even with this construction, one has only merely allocated blame. It is not yet possible to enforce this contract on the Bitcoin blockchain. Bob still trusts Alice not to broadcast an old Commitment Transaction. At this time, he is only able to prove that Alice has done so via a half-signed transaction proof.

然而，即使有這樣的結構，一個人僅僅只是被分配了責任。目前尚不可能在比特幣區塊鏈上執行本合約。Bob 仍然相信 Alice 不會廣播舊承諾交易。在這個時候，他只能夠證明，Alice 這樣做已經通過半簽名的交易證明。

---

### 3.2 創建撤銷合約的通道 | Creating a Channel with Contract Revocation

To be able to actually enforce the terms of the contract, it’s necessary to construct a Commitment Transaction (along with its spends) where one is able to revoke a transaction. This revocation is achievable by using data about when a transaction enters into a blockchain and using the maturity of the transaction to determine validation paths.

為了能夠真正執行合約規定的條款，有必要構建一個承諾交易（連同其支出），為此其中一方就能夠撤銷交易。這個撤銷是可以通過使用交易進入一個區塊鏈資料來實現的，並通過使用該交易的成熟度來確定驗證路徑。

---

### 3.3 nSequence 成熟度 | Sequence Number Maturity

Mark Freidenbach has proposed that Sequence Numbers can be enforcible via a relative block maturity of the parent transaction via a soft-fork[12]. This would allow some basic ability to ensure some form of relative block confirmation time lock on the spending script. In addition, an additional opcode, OP CHECKSEQUENCEVERIFY[13] (a.k.a. OP RELATIVECHECKLOCKTIMEVERIFY)[14], would permit further abilities, including allowing a stop-gap solution before a more permanent solution for resolving transaction malleability. A future version of this paper will include proposed solutions.

Mark Freidenbach 曾提出，nSequence 可以由父交易的相對區塊成熟度通過 Softfork[12] 執行。這將允許一些基本的能力來確保在消費腳本上的某種形式的相對區塊確認時間鏈。此外，額	外	的	操	作	碼	，	OP	CHECKSEQUENCEVERIFY	[13]（	又	名	OP RELATIVECHECKLOCKTIMEVERIFY）[14]，將允許更多的能力，包括允許一個權宜的解決方案，在更長久的解決方案提出之前用於解決交易延展性問題。本文的未來版本將包括提出的解決方案。

---

To summarize, Bitcoin was released with a sequence number which was only enforced in the mempool of unconfirmed transactions. The original behavior permitted transaction replacement by replacing transactions in the mempool with newer transactions if they have a higher sequence number. Due to transaction replacement rules, it is not enforced due to denial of service attack risks. It appears as though the intended purpose of the sequence number is to replace unbroadcasted transactions. However, this higher sequence number replacement behavior is unenforcible. One cannot be assured that old versions of transactions were replaced in the mempool and a block contains the most recent version of the transaction. A way to enforce transaction versions off-chain is via time commitments.

概括地說，具有順序號的比特幣被發行，這些順序號在無確認條件的記憶體池中被執行。原來的行為允許交易置換，如果他們具有較高的 nSequence，可通過在記憶體池中與較新的交易替換。根據交易替換規則，這不是由拒絕服務攻擊風險來執行。nSequence 的預期目的是替代未廣播的交易。然而，這種較高 nSequence 的替換行為是不可執行的。人們不能得到保證，交易的舊版本在記憶體池內已經被替換，一個區塊包含的是最新的版本。一種以 off-chain 形式執行交易版本的方法是通過時間承諾。

---

A Revocable Transaction spends from a unique output where the transaction has a unique type of output script. This parent’s output has two redemption paths where the first can be redeemed immediately, and the second can only be redeemed if the child has a minimum number of confirmations between transactions. This is achieved by making the sequence number of the child transaction require a minimum number of confirmations from the parent. In essence, this new sequence number behavior will only permit a spend from this output to be valid if the number of blocks between the output and the redeeming transaction is above a specified block height. 

可撤銷交易花費從一個獨特輸出中支出，在此獨特的輸出中，交易具有一個獨特類型的輸出 腳本。父交易有 2 條贖回的路徑，其中第一個可以立即贖回，第二個是只有子交易達到一個 最小確認值才可贖回。子交易的 nSequence 的確定需要父交易的最小確認值。從本質上說，這種新的 nSequence 的行為將只能確認從特定輸出的支出是有效的，如果在輸出和贖回交易之間的區塊的數量超過了一個特定的區塊高度。

---

A transaction can be revoked with this sequence number behavior by creating a restriction with some defined number of blocks defined in the sequence number, which will result in the spend being only valid after the parent has entered into the blockchain for some defined number of blocks. 

交易可以通過這些 nSequence 數位行為來贖回，通過一些特定數量的在 nSequence 中確認的區塊創建一個限制，這將導致支出只有在父交易為了一些特定數量的區塊進入區塊鏈之後是有效的。

---

This creates a structure whereby the parent transaction with this output becomes a bonded deposit, attesting that there is no revocation. A time period exists which anyone on the blockchain can refute this attestation by broadcasting a spend immediately after the transaction is broadcast.

這就產生了一個結構，其中父交易和該輸出變成粘結存款，證明沒有撤銷。在一段時間內區塊鏈上的任何人可以通過廣播交易之後立即廣播支出駁斥這種認證。

---

If one wishes to permit revocable transactions with a 1000- confirmation delay, the output transaction construction would remain a 2-of-2 multisig:

2 <Alice 1 > <Bob1> 2 OP CHECKMULTISIG

如果希望允許撤銷交易，這個交易有 1000 個確認延遲，該輸出交易結構將持有 2-of-2 的多信號結構：

***待修正：~~2	<A L I 權證 1> <Bob1> 2 OP CHECKMULTISIG~~***

---

However, the child spending transaction would contain a nSequence value of 1000. Since this transaction requires the signature of both counterparties to be valid, both parties include the nSequence number of 1000 as part of the signature. Both parties may, at their discretion, agree to create another transaction which supersedes that transaction without any nSequence number.

然而，子消費交易將包含 1000 個 nSequence 值，由於該交易需要雙方簽名來確認其有效性，雙方包括 1000 個 nSequence 作為簽名的一部分。雙方當事人可以自行決定，同意創建另一個交易來取代沒有 nSequence 的交易。

This construction, a Revocable Sequence Maturity Contract (RSMC), creates two paths, with very specific contract terms.

這種結構，可撤銷 nSequence 成熟合約（RSMC），通過非常確定的合約條款，創建兩個路徑。

---

The contract terms are:
1.	All parties pay into a contract with an output enforcing this contract
2.	Both parties may agree to send funds to some contract, with some waiting period (1000 confirmations in our example script). This is the revocable output balance.
3.	One or both parties may elect to not broadcast (enforce) the payouts until some future date; either party may redeem the funds after the waiting period at any time.
4.	If neither party has broadcast this transaction (redeemed the funds), they may revoke the above payouts if and only if both parties agree to do so by placing in a new payout term in a superseding transaction payout. The new transaction payout can be immediately redeemed after the contract is disclosed to the world (broadcast on the blockchain).
5.	In the event that the contract is disclosed and the new payout structure is not redeemed, the prior revoked payout terms may be redeemed by either party (so it is the responsibility of either party to enforce the new terms).

該合約的條款是：
1. 所有各方簽訂一份合約，該合約有一個輸出來執行本合約
***2. 雙方當事人同意在一個等待期（在我們的示例腳本中是 1000 個確認）內為一些合約集資，有。這是可撤銷的輸出餘額。***
3. 一方或雙方當事人可以選擇不廣播（執行）的支出，直到將來某個日期;任何一方都可以在等待期後隨時贖回資金。
4. 如果雙方都沒有廣播本次交易（贖回資金），他們可能會撤銷上述支出，當且僅當雙方都同意通過在取代交易支付中放置一個新的支付期限。新的交易支付可以在該合約披露給世界後立即贖回（廣播在區塊鏈上）。
5. 在合約被披露但新的支出結構不贖回的情況下，之前撤銷的支付條款可以由任何一方贖回（所以執行新條款是雙方中任何一方的責任）。

---

The pre-signed child transaction can be redeemed after the parent transaction has entered into the blockchain with 1000 confirmations, due to the child’s nSequence number on the input spending the parent.

預籤子交易可以在父交易已進入有 1000 個確認的區塊鏈之後被贖回，由於子 nSequence 取決於父交易的花費。

---

In order to revoke this signed child transaction, both parties just agree to create another child transaction with the default field of the nSequence number of MAX INT, which has special behavior permitting spending at any time.

為了撤銷這個簽署的子交易，雙方只是同意創建另一個子交易，該子交易的 nSequence 為 MAX INT，它有特殊的行為，允許在任何時候支出。

---

This new signed spend supersedes the revocable spend so long as the new signed spend enters into the blockchain within 1000 confirmations of the parent transaction entering into the blockchain. In effect, if Alice and Bob agree to monitor the blockchain for incorrect broadcast of Commitment Transactions, the moment the transaction gets broadcast, they are able to spend using the superseding transaction immediately. In order to broadcast the revocable spend (deprecated transaction), which spends from the same output as the superseding transaction, they must wait 1000 confirmations. 

只要新簽署的支出進入父交易已經進入的有 1000 個確認的區塊鏈中，這個新簽署的支出將取代可撤銷的花費。事實上，如果 Alice 和 Bob 同意監測區塊鏈，以防其對承諾交易進行不正確廣播，當下交易一廣播，他們能夠立即使用替代交易進行花費。為了廣播可撤銷支出（不建議的交易），其花費與替代交易相同，他們必須等待 1000 個確認。

---

**待處理：~~So long as both parties watch the blockchain, the revocable spend will never enter into the transaction if either party prefers the superseding transaction.~~**


Using this construction, anyone could create a transaction, not broadcast the transaction, and then later create incentives to not ever broadcast that transaction in the future via penalties. This permits participants on the Bitcoin network to defer many transactions from ever hitting the blockchain.

採用這種結構，任何人都可以創建一個交易，不廣播交易，再後來建立激勵機制，使其在未來不通過處罰來廣播交易。這使得比特幣網路上的參與者可以推遲許多在區塊鏈上的交易。

#### 3.3.1 Timestop

To mitigate a flood of transactions by a malicious attacker requires a credible threat that the attack will fail.

要減輕一個惡意的攻擊者製造的信度威脅。

---

Greg Maxwell proposed using a timestop to mitigate a malicious flood on the blockchain:
> There are many ways to address this [flood risk] which haven’t been adequately explored yet —for example, the clock can stop when blocks are full; turning the security risk into more hold-up delay in the event of a dos attack.[15]

Greg Maxwell 提出使用停止狀態以減輕對區塊鏈惡意攻擊：
> 有很多方法可以解決這個問題[洪水風險]，這個問題尚未得到充分的探討-例如，在區塊充足時候時鐘可以停止;在 Dos 攻擊事件[15]發生時，把安全風險轉化為更多的延遲。

---

This can be mitigated by allowing the miner to specify whether the current (fee paid) mempool is presently being flooded with transactions. They can enter a “1” value into the last bit in the version number of the block header. If the last bit in the block header contains a “1”, then that block will not count towards the relative height maturity for the nSequence value and the block is designated as a congested block. There is an uncongested block height (which is always lower than the normal block height). This block height is used for the nSequence value, which only counts block maturity (confirmations).

這可以通過讓礦工確認現有的（手續費支付）記憶體池目前是否交易氾濫來得到緩解。他們可以輸入一個“1”值到塊標題的版本號的最後一位。如果在塊標題的最後一位包含一個“1”，則該塊將不計入的相對成熟高度的 nSequence 值，並且該區塊被指定為一個擁擠區塊。有一不擁擠區塊高度（它總是比正常塊高度低）。此區塊高度用於確定 nSequence 價值，這只能算作區塊成熟（確認條件）。

---

A miner can elect to define the block as a congested block or not. The default code could automatically set the congested block flag as “1” if the mempool is above some size and the average fee for that set size is above some value. However, a miner has full discretion to change the rules on what automatically sets as a congested block, or can select to permanently set the congestion flag to be permanently on or off. It’s expected that most honest miners would use the default behavior defined in their miner and not organize a 51% attack.

一名礦工可以選擇區塊是否擁擠。如果記憶體池大於一定的規模或者對於確定大小的記憶體池的平均手續費大於一定的值，默認代碼可以自動設置擁擠區塊為“1”。然而，一個礦工有完全的決定權來改變確定自動設置為擁擠塊的規則，或者可以選擇是否設置為永久的擁擠。最誠實的礦工將使用默認的行為去定義他們的礦工，而不是組織一次 51％ 的攻擊。

---

For example, if a parent transaction output is spent by a child with a nSequence value of 10, one must wait 10 confirmations before the transaction becomes valid. However, if the timestop flag has been set, the counting of confirmations stops, even with new blocks. If 6 confirmations have elapsed (4 more are necessary for the transaction to be valid), and the timestop block has been set on the 7th block, that block does not count towards the nSequence requirement of 10 confirmations; the child is still at 6 blocks for the relative confirmation value.
Functionally, this will be stored as some kind of auxiliary timestop block height which is used only for tracking the timestop value. When the timestop bit is set, all transactions using an nSequence value will stop counting until the timestop bit has been unset. This gives sufficient time and block-space for transactions at the current auxiliary timestop block height to enter into the blockchain, which can prevent systemic attackers from successfully attacking the system.

例如，如果一個父交易輸出由一個 nSequence 值為 10 的子交易花費，在交易生效之前我們必須等待 10 次確認。然而，如果 timestop 已經確定，即使採用新的區塊，計算的確認也應 當停止。如果 6 次確認已經完成（再需要 4 次確認交易才是有效的），並且 timestop 區塊已設置的第七區塊上，該塊不要求 10 次 nSequence 的確認，孩子目前仍處於第 6 區塊相對確認值。在功能上，這將被儲存為某種輔助 timestop 區塊高度，僅用於跟蹤 timestop 值。當 timestop 位數已經設置，使用 nSequence 值的所有交易將停止計數，直到 timestop 位數恢復未設置狀態。這給當前輔助 timestop 區塊高度中的交易提供了充分的時間和區塊空間來進入區塊鏈，它可以防止系統攻擊者成功地攻擊系統。

---

However, this requires some kind of flag in the block to designate whether it is a timestop block. For full SPV compatibility (Simple Payment Verification; lightweight clients), it is desirable for this to be within the 80 byte block header instead of in the coinbase. There are two places which may be a good place to put in this flag in the block header: in the block time and in the block version. The block time may not be safe due to the last bits being used as an entropy source for some ASIC miners, therefore a bit may need to be consumed for timestop flags. Another option would be to hardcode timestop activation as a hard consensus rule (e.g. via block size), however this may make things less flexible. By setting sane defaults for timestop rules, these rules can be changed without consensus soft-forks.

然而，這需要區塊中的某種標誌指定它是否是一個 timestop 區塊。對於 SPV 完全相容性（簡單付款確認;羽量級用戶端），它要求在 80 Byte的區塊標頭內，而不是在 coinbase。在區塊標頭存放這一標誌的可能的兩個地方：區塊時間和區塊版本。區塊時間可能不安全，由於最後一位被一些 ASIC 的礦工用作熵源，因此可能需要一位元被消耗用於 timestop 標誌。另一種選擇是硬編碼 timestop 啟動，作為硬協商一致規則（例如，通過區塊大小），但是這可能使 事情變得不太靈活。通過設置 timestop 的健全的預設規則，這些規則可以不通過一致的 soft-forks 來改變。

---

If the block version is used as a flag, the contextual information must match the Chain ID used in some merge-mined coins.

如果區塊的版本被用作標誌，上下文資訊必須以某種合併開採硬幣中使用的鏈 ID 相匹配。

---

#### 3.3.2 撤銷承諾交易 | Revocable Commitment Transactions

By combining the ascribing of blame as well as the revocable transaction, one is able to determine when a party is not abiding by the terms of the contract, and enforce penalties without trusting the counterparty.

通過結合錯誤來源以及可撤銷交易，能夠確定什麼時候一方不遵守合約的條款，並不信任對方的實施處罰。

---

![](image/figure4.png)

Figure 4: The Funding Transaction F, designated in green, is broadcast on the blockchain after all other transactions are signed. All transactions which only Alice can broadcast are in purple. All transactions which only Bob can broadcast is are blue. Only the Funding Transaction is broadcast on the blockchain at this time.

圖 4：資金交易 F，用綠色代表，在所有其他交易簽署之後在區塊鏈上廣播。所有交易只有 Alice 可以廣播的交易由紫色代表。所有只有 Bob 可以廣播的交易由藍色代表。此時，只有交易資金在區塊鏈上廣播。

---

The intent of creating a new Commitment Transaction is to invalidate all old Commitment Transactions when updating the new balance with a new Commitment Transaction. Invalidation of old transactions can happen by making an output be a Revocable Sequence Maturity Contract (RSMC). To invalidate a transaction, a superseding transaction will be signed and exchanged by both parties that gives all funds to the counterparty in the event an older transaction is incorrectly broadcast. The incorrect broadcast is identified by creating two different Commitment Transactions with the same final balance outputs, however the payment to oneself is encumbered by an RSMC.

創建一個新的承諾交易的目的是在利用新的承諾交易來更新新的餘額，使所有的舊的承諾交易無效。要使舊的交易失效，要使輸出成為可撤銷的序列到期合約（RSMC）。要使交易無效，將簽署一個替代的交易，並且雙方交換此交易，規定雙方在不正確的廣播舊交易的情況下將資金交給對方。不正確的廣播通過創建具有相同的網路最終餘額輸出的兩個不同承諾交易來鑒定，但是給自己的支付由 RSMC 擔保。

---

In effect, there are two Commitment Transactions from a single Funding Transaction 2-of-2 outputs. Of these two Commitment Transactions, only one can enter into the blockchain. Each party within a channel has one version of this contract. So if this is the first Commitment Transaction pair, Alice’s Commitment Transaction is defined as C1a, and Bob’s Commitment Transaction is defined as C1b. By broadcasting a Commitment Transaction, one is requesting for the channel to close out and end. The first two outputs for the Commitment Transaction include a Delivery Transaction (payout) of the present unallocated balance to the channel counterparties. If Alice broadcasts C1a, one of the output is spendable by D1a, which sends funds to Bob. For Bob, C1b is spendable by D1b, which sends funds to Alice. The Delivery Transaction (D1a/D1b) is immediately redeemable and is not encumbered in any way in the event the Commitment Transaction is broadcast.

實際上，2-of-2 資金交易輸出有兩個承諾交易。這兩個承諾交易中，只有一個可以進入到 區塊鏈。通道內的每一方都有本合約的一個版本。因此，如果這是第一個承諾交易對， Alice 的承諾交易被定義為 C1a， Bob 的承諾交易被定義為 C1b。若要廣播一個承諾交易，要求的通道關閉並結束。承諾交易的前兩個輸出包括目前未分配的與通道對方為分配餘額的交付交易（派息）。如果 Alice 廣播 C1a，其中一個輸出對 D1a 是可支配的，它發送資金給 Bob。 Bob，C1b 的是可由 D1b 支配的，它發送資金給 Alice。該交付交易（D1a / D1b）是被立即贖回的，並以任何方式廣播交易承諾不受到阻礙。

---

For each party’s Commitment Transaction, they are attesting that they are broadcasting the most recent Commitment Transaction which they own. Since they are attesting that this is the current balance, the balance paid to the counterparty is assumed to be true, since one has no direct benefit by paying some funds to the counterparty as a penalty.

對於每一方的承諾交易，他們證明他們正在廣播他們擁有最新的承諾交易。因為他們證明，這是當前餘額，支付給對方的餘額被認為是真實的，因為作為一種懲罰向對方支付資金對自己是沒有任何直接好處的。

---

The balance paid to the person who broadcast the Commitment Transaction, however, is unverified. The participants on the blockchain have no idea if the Commitment Transaction is the most recent or not. If they do not broadcast their most recent version, they will be penalized by taking all the funds in the channel and giving it to the counterparty. Since their own funds are encumbered in their own RSMC, they will only be able to claim their funds after some set number of confirmations after the Commitment Transaction has been included in a block (in our example, 1000 confirmations). If they do broadcast their most recent Commitment Transaction, there should be no revocation transaction superseding the revocable transaction, so they will be able to receive their funds after some set amount of time (1000 confirmations).

將餘額支付給廣播承諾交易的人是未確認的。區塊鏈上的參與者不知道承諾交易是否是最近的。如果他們沒有廣播他們的最新版本，他們將被懲罰，承擔通道中所有的資金並給與交易對方。由於自己的資金都押在自己的 RSMC 中，他們只能在承諾交易已被列入一個區塊後（在我們的例子中，1000 次確認），經過一定數量的確認後要求自己的資金。如果他們廣播的是自己的最新承諾交易，應該沒有撤銷交易替換之前可撤銷的交易，所以他們就能夠在一段時間（1000 次確認）後取回投入的資金。

---

By knowing who broadcast the Commitment Transaction and encumbering one’s own payouts to be locked up for a predefined period of time,both parties will be able to revoke the Commitment Transaction in the future.

通過瞭解誰廣播承諾交易，並阻礙自己的支出在提前確定好的的時間內被鎖定，雙方將能夠在未來撤銷承諾交易。

---

#### 3.3.3 從通道兌換資金：合作交易方 | Redeeming Funds from the Channel: Cooperative Counterparties

Either party may redeem the funds from the channel. However, the party that broadcasts the Commitment Transaction must wait for the predefined number of confirmations described in the RSMC. The counterparty which did not broadcast the Commitment Transaction may redeem the funds immediately.

任何一方都可以從通道贖回資金。然而，廣播承諾交易的一方必須等待在 RSMC 描述的提前確定好的交易數量。沒有廣播的承諾交易的交易對方可以立即贖回資金。

---

For example, if the Funding Transaction is committed with 1 BTC (half to each counterparty) and Bob broadcasts the most recent Commitment Transaction, C1b, he must wait 1000 confirmations to receive his 0.5 BTC, while Alice can spend 0.5 BTC. For Alice, this transaction is fully closed if Alice agrees that Bob broadcast the correct Commitment Transaction (C1b).

例如，如果資金交易承諾 1 BTC（每個交易對方一半），並且 Bob 廣播最新的承諾交易 C1b，他必須等 1000 次確認才能得到他的 0.5 BTC，Alice 可以花費 0.5 BTC。對於 Alice，本次交 易是完全封閉的，如果 Alice 同意 Bob 廣播的承諾交易（C1b）是正確的。

---

![](image/figure5.png)

Figure 5: When Bob broadcasts C1b, Alice can immediately redeem her portion. Bob must wait 1000 confirmations. When the block is immediately broadcast, it is in this state. Transactions in green are transactions which are committed into the blockchain.

圖 5：當 Bob 廣播 C1b 時，Alice 可以立即贖回她的部分。Bob 必須等到 1000 次確認。當區塊被立即廣播，它是在該狀態下。綠色交易是它們提交到區塊鏈的交易。

---

After the Commitment Transaction has been in the blockchain for 1000 blocks, Bob can then broadcast the Revocable Delivery transaction. He must wait 1000 blocks to prove he has not revoked this Commitment Transaction (C1b). After 1000 blocks, the Revocable Delivery transaction will be able to be included in a block. If a party attempt to include the Revocable Delivery transaction in a block before 1000 confirmations, the transaction will be invalid up until after 1000 confirmations have passed (at which point it will become valid if the output has not yet been redeemed).

承諾交易已經在區塊鏈1000 區塊之後，Bob 就可以廣播可撤銷的交付交易。他必須等到 1000 區塊，以證明他並沒有撤銷該承諾交易（C1b）。1000 區塊後，可撤銷的交付交易將能夠被包括在一個區塊中。如果一方企圖包括在 1000 次確認之前將可撤銷的交付交易納入區塊，1000 次確認後該交易將是無效的（如果輸出尚未贖回，此時它就會成為有效的）。

---

![](image/figure6.png)

Figure 6: Alice agrees that Bob broadcast the correct Commitment Transaction and 1000 confirmations have passed. Bob then is able to broadcast the Revocable Delivery (RD1b) transaction on the blockchain.

圖 6：Alice 同意，Bob 廣播正確的承諾交易並且 1000 次確認已經過去了。Bob 能夠在
區塊鏈上廣播可撤銷交付交易（RD1b）。

---

After Bob broadcasts the Revocable Delivery transaction, the channel is fully closed for both Alice and Bob, everyone has received the funds which they both agree are the current balance they each own in the channel.

Bob 廣播可撤銷交貨的交易後，對於 Alice 和 Bob，該通道完全關閉，每個人都收到了資金，他們都同意在當前餘額下，他們在通道內分別擁有的資金。

---

If it was instead Alice who broadcast the Commitment Transaction (C1a), she is the one who must wait 1000 confirmations instead of Bob.

如果是 Alice 廣播承諾交易（C1a），她必須等到 1000 次確認，而不是Bob。

---

#### 3.3.4 創建一個新的交易承諾，並撤銷先前的承諾 | Creating a new Commitment Transaction and Revoking Prior Commitments

While each party may close out the most recent Commitment Transaction at any time, they may also elect to create a new Commitment Transaction and invalidate the old one.

雖然任何一方都可以在任何時候收回最近交易承諾，他們也可以選擇創建一個新的承諾交易並且使舊的交易無效。

---

Suppose Alice and Bob now want to update their current balances from 0.5 BTC each refunded to 0.6 BTC for Bob and 0.4 BTC for Alice.When they both agree to do so, they generate a new pair of Commitment Transactions.

假設 Alice 和 Bob 現在要更新每人 0.5 BTC 的餘額，並且退還 0.6 BTC 給 Bob 和 0.4 BTC 給 Alice。當他們都同意這樣做，它們產生了一對新承諾交易。

---

![](image/figure7.png)

Figure 7: Four possible transactions can exist, a pair with the old commitments, and another pair with the new commitments. Each party inside the channel can only broadcast half of the total commitments (two each). There is no explicit enforcement preventing any particular Commitment being broadcast other than penalty spends, as they are all valid unbroadcasted spends. The Revocable Commitment still exists with the C1a/C1b pair, but are not displayed for brevity.

圖 7：四種可能的交易可以存在，一對舊的承諾，另一對新的承諾。通道內每一方只能廣播一半的承諾。沒有明確的執行防止任何特定的承諾被廣播而不是懲罰花費，因為它們都是有效的未廣播的花費。可撤銷的承諾仍與 C1a / C1b 成對存在，但不顯示簡短。

---

When a new pair of Commitment Transactions (C2a/C2b) is agreed upon, both parties will sign and exchange signatures for the new Commitment Transaction, then invalidate the old Commitment Transaction. This invalidation occurs by having both parties sign a Breach Remedy Transaction (BR1), which supersedes the Revocable Delivery Transaction (RD1). Each party hands to the other a half-signed revocation (BR1) from their own Revocable Delivery (RD1), which is a spend from the Commitment Transaction. The Breach Remedy Transaction will send all coins to the counterparty within the current balance of the channel. For example, if Alice and Bob both generate a new pair of Commitment Transactions (C2a/C2b) and invalidate prior commitments (C1a/C1b), and later Bob incorrectly broadcasts C1b on the blockchain, Alice can take all of Bob’s money from the channel. Alice can do this because Bob has proved to Alice via penalty that he will never broadcast C1b, since the moment he broadcasts C1b, Alice is able to take all of Bob’s money in the channel. In effect, by constructing a Breach Remedy transaction for the counterparty, one has attested that one will not be broadcasting any prior commitments. The counterparty can accept this, because they will get all the money in the channel when this agreement is violated.

當一個新的對交易的承諾（C2A / C2b）達成一致，雙方將簽署並交換新承諾交易的簽名，然後舊的承諾交易失效。這種失效通過讓雙方簽署違約補救交易（BR1）發生，它取代了撤銷交付交易（RD1）。每一方從自己的撤銷交付（RD1）發送給另一方的簽訂一半的撤銷交易（BR1），這是承諾交易的花費。違約補救交易就會把通道現有餘額中所有的現金給對方。例如，如果 Alice 和 Bob 都產生了一對新承諾交易（C2A / C2b）和失效的舊的承諾（C1a / C1b），後來 Bob 在區塊鏈不正確的廣播 C1b，Alice 可以拿走通道中 Bob 所有的錢。 Alice 能做到這一點，因為 Bob 已經通過懲罰向 Alice 證明，他將永遠不會廣播 C1b，因為他廣播 C1b 的那一刻，Alice 可以拿走通道中 Bob 所有的錢。事實上，通過為對方構建違約補救交易，一方已經證明，不會廣播任何事先的承諾。對方可以接受這一點，因為若該協議被違反，他們將得到通道中所有的錢。

---

![](image/figure8.png)

Figure 8: When C2a and C2b exist, both parties exchange Breach Remedy transactions. Both parties now have explicit economic incentive to avoid broadcasting old Commitment Transactions (C1a/C1b). If either party wishes to close out the channel, they will only use C2a (Alice) or C2b (Bob). If Alice broadcasts C1a, all her money will go to Bob. If Bob broadcasts C1b, all his money will go to Alice. See previous figure for C2a/C2b outputs.

圖 8：當 C2A 和 C2B 存在，雙方交換違約補救交易。雙方現在都有明確的經濟激勵，避免廣播舊的承諾交易（C1a / C1b）。如一方要求關閉通道，他們將只使用 C2A（Alice）或 C2b 上（BOB）。如果 Alice 廣播 C1a，所有的錢都會給 Bob。如果 Bob 廣播 C1b，所有的錢都會給 Alice。C2A / C2b 的輸出請參閱前面的數位。

---

Due to this fact, one will likely delete all prior Commitment Transactions when a Breach Remedy Transaction has been passed to the counterparty. If one broadcasts an incorrect (deprecated and invalidated Commitment Transaction), all the money will go to one’s counterparty. For example, if Bob broadcasts C1b, so long as Alice watches the blockchain within the predefined number of blocks (in this case, 1000 blocks), Alice will be able to take all the money in this channel by broadcasting RD1b. Even if the present balance of the Commitment state (C2a/C2b) is 0.4 BTC to Alice and 0.6 BTC to Bob, because Bob violated the terms of the contract, all the money goes to Alice as a penalty. Functionally, the Revocable Transaction acts as a proof to the blockchain that Bob has violated the terms in the channel and this is programatically adjudicated by the blockchain.

由於這一事實，當違約補救交易已經交給交易對方時，人們可能會刪除所有先前的承諾交易。如果一方廣播不正確（過時的，無效的承諾交易），所有的錢都會給對方。例如，如果 Bob 廣播 C1b，只要 Alice 在事先定好的區塊數量範圍內觀察區塊鏈（在此情況下，1000 區 塊），Alice 將能夠通過廣播 RD1b 得到在這個通道的所有的錢。即使當前餘額的承諾狀態
（C2A / C2b）為 0.4 BTC 給 Alice 和 0.6 BTC 給 Bob，因為 Bob 違反了合約條款，作為懲罰，所有的錢給 Alice。在功能上，可撤銷交易作為一個區塊鏈上的證明，證明 Bob 違反渠道中的條款，並且這是由區塊鏈程式設計判定的。

---

![](image/figure9.png)

Figure 9: Transactions in green are committed to the blockchain. Bob incorrectly broadcasts C1b (only Bob is able to broadcast C1b/C2b). Because both agreed that the current state is the C2a/C2b Commitment pair, and have attested to each party that old commitments are invalidated via Breach Remedy Transactions, Alice is able to broadcast BR1b and take all the money in the channel, provided she does it within 1000 blocks after C1b is broadcast.

圖 9：綠色代表被提交到區塊鏈上的交易。Bob 錯誤得廣播 C1b（只有 Bob 能夠廣播 C1b / C2b）。由於雙方都同意目前的狀態是 C2A / C2b 承諾對，並且已經通過違約補救交易證明了舊的承諾是無效的，Alice 能夠廣播 BR1b，並得到通道中所有的錢，只要她在 C1b 廣播 1000 區塊以後來執行。

---

However, if Alice does not broadcast BR1b within 1000 blocks, Bob may be able to steal some money, since his Revocable Delivery Transaction (RD1b) becomes valid after 1000 blocks. When an incorrect Commitment Transaction is broadcast, only the Breach Remedy Transaction can be broadcast for 1000 blocks (or whatever number of confirmations both parties agree to). After 1000 block confirmations, both the Breach Remedy (BR1b) and Revocable Delivery Transactions (RD1b) are able to be broadcast at any time. Breach Remedy transactions only have exclusivity within this predefined time period, and any time after of that is functionally an expiration of the statute of limitations —according to Bitcoin blockchain consensus, the time for dispute has ended.

但是，如果 Alice 不在 C1b 廣播 1000 區塊以後廣播 BR1b，Bob 也許能偷一些錢，因為他的撤銷交付交易（RD1b）在 1000 區塊後有效。當一個不正確的交易承諾被廣播，只有違約補救交易可廣播 1000 區塊（或其他的雙方同意的確認數量）。經過 1000 次確認，無論是違約補救措施（BR1b）還是可撤銷的交付交易（RD1b）能夠在任何時間廣播。違約補救交易只有在這個提前定義的時間段內具有排他性，之後的任何時間在功能上受到限制-根據比特幣區塊鏈共識，爭論的時間已經結束。

---

For this reason, one should periodically monitor the blockchain to see if one’s counterparty has broadcast an invalidated Commitment Transaction, or delegate a third party to do so. A third party can be delegated by only giving the Breach Remedy transaction to this third party. They can be incentivized to watch the blockchain broadcast such a transaction in the event of counterparty maliciousness by giving these third parties some fee in the output. Since the third party is only able to take action when the counterparty is acting maliciously, this third party does not have any power to force close of the channel.

為此，應定期監測區塊鏈，監控其對方是否廣播了無效的承諾交易，或委託協力廠商這樣做。只能通過向這個協力廠商提供違約補救交易來對其進行委託。協力廠商被激勵去監控區塊鏈中這樣的對方惡意廣播的交易，通過給這些協力廠商一些輸出中的手續費。由於第三人只能在對方惡意行為時採取行動，該協力廠商沒有任何強制關閉通道的權力。

---

#### 3.3.5 創建可撤銷承諾交易流程 | Process for Creating Revocable Commitment Transactions

To create revocable Commitment Transactions, it requires proper construction of the channel from the beginning, and only signing transactions which may be broadcast at any time in the future, while ensuring that one will not lose out due to uncooperative or malicious counterparties. This requires determining which public key to use for new commitments, as using SIGHASH NOINPUT requires using unique keys for each Commitment Transaction RSMC (and HTLC) output. We use P to designate pubkeys and K to designate the corresponding private key used to sign.

要創建可撤銷承諾交易，首先它需要正確的結構通道，並且僅僅簽訂可能在未來任何時間公布的交易，同時確保一方不會因不合作或惡意的對方而吃虧。這需要確定新的承諾要使用的公開金鑰，如使用 SIGHASH NOINPUT 要求每一個承諾交易 RSMC（和 HTLC）輸出使用特殊 鑰。我們用 P 來指定公開金鑰，K 來指定用於簽署的相應的私密金鑰。

---

When generating the first Commitment Transaction, Alice and Bob agree to create a multisig output from a Funding Transaction with a single multisig(PAliceF , PBobF ) output, funded with 0.5 BTC from Alice and Bob for a total of 1 BTC. This output is a Pay to Script Hash[16] transaction, which requires both Alice and Bob to both agree to spend from the Funding Transaction. They do not yet make the Funding Transaction (F) spendable. Additionally, PAliceF and PBobF are only used for the Funding Transaction, they are not used for anything else.

當生成第一個承諾交易時，Alice 和 Bob 同意從一個資金交易一具有單一 multisig（PAliceF，PBobF）輸出中創建一個 multisig 輸出，提供來自 Alice 和 Bob 各 0.5 BTC，共 1 BTC 的資金。該輸出是一種給雜湊腳本[16]交易的付費，這需要雙方 Alice 和 Bob 同意從交易資金花費。他們還沒有使資金交易（F）可支配。此外，PAliceF 和 PBobF 僅用於資金交易，它們不用於其他任何東西。

---

Since the Delivery transaction is just a P2PKH output (bitcoin addresses beginning with 1) or P2SH transaction (commonly recognized as addresses beginning with the 3) which the counterparties designate beforehand, this can be generated as an output of PAliceD and PBobD . For simplicity, these output addresses will remain the same throughout the channel, since its funds are fully controlled by its designated recipient after the Commitment Transaction enters the blockchain. If desired, but not necessary, both parties may update and change PAliceD and PBobD for future Commitment Transactions.

由於交付交易僅僅是一個 P2PKH 輸出（比特幣地址從 1 開始）或 P2SH 交易（通常認為地址從 3 開始），這需要對方事先指定，這可以由 PAliceD 和 PBobD 的輸出生成。簡單起見，這些輸出地址將在整個通道過程保持不變，因為其資金是由它的指定接收方的承諾交易進入區塊鏈後完全控制。如果需要，但不是必要的話，雙方可以為未來的承諾交易更新改變 PAliceD 和 PBobD。

---

Both parties exchange pubkeys they intend to use for the RSMC (and HTLC described in future sections) for the Commitment Transaction. Each set of Commitment Transactions use their own public keys and are not ever reused. Both parties may already know all future pubkeys by using a BIP 0032[17] HD Wallet construction by exchanging Master Public Keys during channel construction. If they wish to generate a new Commitment Transaction pair C2a/C2b, they use multisig(PAliceRSMC2, PBobRSMC2) for the RSMC output.

雙方交換他們打算為承諾交易的 RSMC（和在以後的章節描述的 HTLC）使用的公開金鑰。每套承諾交易用自己的公開金鑰並且永遠不重複使用。雙方可能已經知道未來所有的公開金鑰，通過使用 BIP 0032 [17] HD 錢包結構通道過程中交換主公共金鑰。如果他們希望生成一個新的承諾交易對 C2A / C2b，他們為 RSMC 輸出使用 multisig（PAliceRSMC2，PBobRSMC2）。

---

After both parties know the output values from the Commitment Transactions, both parties create the pair of Commitment Transactions,
e.g. C2a/C2b, but do not exchange signatures for the Commitment Transactions. They both sign the Revocable Delivery transaction (RD2a/RD2b) and exchange the signatures. Bob signs RD1a and gives it to Alice (using KBobRSMC2), while Alice signs RD1b and gives it to Bob (using KAliceRSMC2).

雙方都知道承諾交易的輸出值之後，雙方建立了交易承諾對，如 C2A / C2b，但不為承諾交 易交換簽名。他們都簽署撤銷交付交易（RD2a / RD2b），並交換了簽名。Bob 簽署 RD1a並將其交給 Alice （ 使 用 KBobRSMC2 ） ， Alice 簽 名 RD1b 並 將 其 交 給 Bob （ 使 用 KAliceRSMC2）。

---

When both parties have the Revocable Delivery transaction, they ex-
change signatures for the Commitment Transactions. Bob signs C1a using KBobF and gives it to Alice, and Alice signs C1b using KAliceF and gives it to Bob.

當雙方都有可撤銷的交付交易時，它們為承諾交易交換簽名。Bob 使用 KBobF 簽署 C1a 並將其交給 Alice， Alice 使用 KAliceF 簽署 C1b 並將其交給 Bob。

---

At this point, the prior Commitment Transaction as well as the new Commitment Transaction can be broadcast; both C1a/C1b and C2a/C2b are valid. (Note that Commitments older than the prior Commitment are invalidated via penalties.) In order to invalidate C1a and C1b, both parties exchange Breach Remedy Transaction (BR1a/BR1b) signatures for the prior commitment C1a/C1b. Alice sends BR1a to Bob using KAliceRSMC1, and Bob sends BR1b to Alice using KBobRSMC1. When both Breach Remedy signatures have been exchanged, the channel state is now at the current Commitment C2a/C2b and the balances are now committed.

在這一點上，先前的承諾交易以及新的承諾交易能夠被廣播; C1a / C1b 和 C2A / C2b 上都是有效的。（注意，早於先前承諾的承諾通過處罰被判定為無效的。）為了使 C1a 和 C1b 無效，雙方為先前承諾 C1a / C1b 交換違約補救交易（ BR1a / BR1b）簽名。 Alice 使用 KAliceRSMC1 發送 BR1a 給 Bob，Bob 使用 KBobRSMC1 發送 BR1b 給 Alice。當兩個違約補救簽名進行了交換，通道狀態是在當前承諾 C2A / C2b 上的餘額。

---

However, instead of disclosing the BR1a/BR1b signatures, it’s also possible to just disclose the private keys to the counterparty. This is more effective as described later in the key storage section. One can disclose the private keys used in one’s own Commitment Transaction. For example, if Bob wishes to invalidate C1b, he sends his private keys used in C1b to Alice (he does NOT disclose his keys used in C1a, as that would permit coin theft). Similarly, Alice discloses all her private key outputs in C1a to Bob to invalidate C1a.

然而，不公開 BR1a / BR1b 簽名，也可能只是透露私密金鑰給對方。在後面介紹的金鑰儲存部分會說明這樣是更有效率的。一方可以公開在自己的承諾交易中使用的私有金鑰。例如，如果 Bob 希望使 C1b 無效，他將他用於 C1b 的私密金鑰發送給 Alice（他沒有透露他在 C1a 使用的密鑰，因為這將引起硬幣盜竊）。同樣，Alice 向 Bob 公開了她在 C1a 所有的私有金鑰來使 C1a 無效。

---

If Bob incorrectly broadcasts C1b, then because Alice has all the private keys used in the outputs of C1b, she can take the money. However, only Bob is able to broadcast C1b. To prevent this coin theft risk, Bob should destroy all old Commitment Transactions.

如果 Bob 沒有正確廣播 C1b，因為 Alice 有所有的 C1b 輸出使用的私密金鑰，她可以拿錢。然而，只有 Bob 能夠廣播 C1b。為了防止這種硬幣被盜風險，Bob 應該銷毀所有舊的承諾交易。

---

### 3.4 協同關閉通道 | Cooperatively Closing Out a Channel

Both parties are able to send as many payments to their counterparty as they wish, as long as they have funds available in the channel, knowing that in the event of disagreements they can broadcast to the blockchain the current state at any time.

雙方都能夠按照自己的意願來發送任何數量的支付給他們的對方，只要他們在通道有可用資金，因為他們知道在意見分歧的情況下，他們可以在任何時間在區塊鏈上廣播當前的狀態。

---

In the vast majority of cases, all the outputs from the Funding Transaction will never be broadcast on the blockchain. They are just there in case the other party is non-cooperative, much like how a contract is rarely enforced in the courts. A proven ability for the contract to be enforced in a deterministic manner is sufficient incentive for both parties to act honestly.

在絕大多數情況下，從資金交易所有輸出將永遠不會在區塊鏈廣播。他們只是在對方不合作的情況下出現，很像一份在法庭上很少執行的合約。該合約被證明有能力以一個確定的方式來強制執行有效地激勵雙方誠實守信。

---

When either party wishes to close out a channel cooperatively, they will be able to do so by contacting the other party and spending from the Funding Transaction with an output of the most current Commitment Transaction directly with no script encumbering conditions. No further payments may occur in the channel.

當一方希望關閉通道，他們將能夠這樣做，通過與對方創建合約，從現有的承諾交易不通過腳本阻礙條件直接花費。在通道中沒有進一步的付款可能發生。

---

![](image/figure10.png)

Figure 10: If both counterparties are cooperative, they take the balances in the current Commitment Transaction and spend from the Funding Transaction with a Exercise Settlement Transaction (ES). If the most recent Commitment Transaction gets broadcast instead, the payout (less fees) will be the same.

圖 10：如果雙方是合作的，他們採取當前交易承諾的餘額，並從有運用結算交易（ES）的資金交易中花費。如果最近的承諾交易被廣播，支出（較少手續費）將是相同的。

---

The purpose of closing out cooperatively is to reduce the number of transactions that occur on the blockchain and both parties will be able to receive their funds immediately (instead of one party waiting for the Revocation Delivery transaction to become valid).

合作關閉通道的目的是為了減少發生在區塊鏈上的交易數量，雙方將能夠立即收到他們的資金（而不是一方等待撤銷交付交易有效）。

---

Channels may remain in perpetuity until they decide to cooperatively close out the transaction, or when one party does not cooperate with another and the channel gets closed out and enforced on the blockchain.

通道可永久存在，直到他們決定合作關閉交易，或當一方不與另一方相互合作，在區塊鏈上執行關閉通道。

---

### 3.5 雙向通道的啟示與總結 | Bidirectional Channel Implications and Summary

By ensuring channels can update only with the consent of both parties, it is possible to construct channels which perpetually exist in the blockchain. Both parties can update the balance inside the channel with whatever output balances they wish, so long as it’s equal or less than the total funds committed inside the Funding Transaction; balances can move in both directions. If one party becomes malicious, either party may immediately close out the channel and broadcast the most current state to the blockchain. By using a fidelity bond construction (Revocable Delivery Transactions), if a party violates the terms of the channel, the funds will be sent to the counterparty, provided the proof of violation (Breach Remedy Transaction) is entered into the blockchain in a timely manner. If both parties are cooperative, the channel can remain open indefinitely, possibly for many years.

通過確保通道只能在雙方當事人的同意的情況下得到更新，就可以構建永遠存在於區塊鏈上的通道。雙方可以在通道內以他們所希望的輸出更新餘額，只要它是等於或小於承諾資金交易內的資金總額;餘額可以在兩個方向上移動。如果一方是惡意的，任何一方都可以立即關閉通道並且廣播最新狀態到區塊鏈。通過使用網路保真債券建築（撤銷交付交易），如果一方當事人違反的通道的條款，資金將被發送給對方，只要違反（違約補救交易）的證明及時進入區塊鏈。如果雙方是合作，通道可以保持無限期地打開，可能很多年。

---

This type of construction is only possible because adjudication occurs programatically over the blockchain as part of the Bitcoin consensus, so one does not need to trust the other party. As a result, one’s channel counterparty does not possess full custody or control of the funds.

這種類型的結構是唯一可能的，因為審判程式設計作為比特幣共識的一部分發生在 區塊鏈上，所以人們並不需要信任對方。這樣一來，一方的通道對方不能充分的監管或控制資金。

---

## 4 雜湊 Timelock 合約（HTLC）| Hashed Timelock Contract (HTLC)

A bidirectional payment channel only permits secure transfer of funds inside a channel. To be able to construct secure transfers using a network of channels across multiple hops to the final destination requires an additional construction, a Hashed Timelock Contract (HTLC).

雙向支付通道只允許資金在通道內安全轉移。為了能夠使用跨多個中繼段通向網路最終目的地得網路通道建立安全傳輸，需要一個額外的結構，雜湊 Timelock 合約（HTLC）。

---

The purpose of an HTLC is to allow for global state across multiple nodes via hashes. This global state is ensured by time commitments and time-based unencumbering of resources via disclosure of preimages. Transactional “locking” occurs globally via commitments, at any point in time a single participant is responsible for disclosing to the next participant whether they have knowledge of the preimage R. This construction does not require custodial trust in one’s channel counterparty, nor any other participant in the network.

一個 HTLC 的目的是通過雜湊允許在多個節點的全域狀態。這種全域狀態通過披露原像由承諾的時間和以時間為基準的無阻礙資源來確保。交易“鎖定”通過承諾發生在全域，在特定時間，一個參與者負責披露給下一參與者他們是否掌握原像 R 的資訊。這種結構並不要求通道中對方的保管信託，在網路中也沒有任何其他參與者。

---

In order to achieve this, an HTLC must be able to create certain transactions which are only valid after a certain date, using nLockTime, as well as information disclosure to one’s channel counterparty. Additionally, this data must be revocable, as one must be able to undo an HTLC.

為了達到這個，一個 HTLC 必須能夠創建只在特定日期後有效的某些交易，使用 nLockTime，以及公開給通道對方的資訊。此外，該資料必須是可撤銷的，因為一個人必須能夠撤銷 HTLC。

---

An HTLC is also a channel contract with one’s counterparty which is enforcible via the blockchain. The counterparties in a channel agree to the following terms for a Hashed Timelock Contract:

1.	If Bob can produce to Alice an unknown 20-byte random input data R from a known hash H, within three days, then Alice will settle the contract by paying Bob 0.1 BTC.
2.	If three days have elapsed, then the above clause is null and void and the clearing process is invalidated, both parties must not attempt to settle and claim payment after three days.
3.	Either party may (and should) pay out according to the terms of this contract in any method of the participants choosing and close out this contract early so long as both participants in this contract agree.
4.	Violation of the above terms will incur a maximum penalty of the funds locked up in this contract, to be paid to the non-violating counterparty as a fidelity bond.

HTLC 也是一個可在區塊鏈上執行的與對方簽訂的的通道合約。通道中對方同意雜湊
Timelock 合約的以下條款：

1. 如果 Bob 可以為 Alice 從已知的雜湊值 H 中產生未知的 20 Byte的的隨機輸入資料 R，在三天之內，Alice 將通過支付 Bob0.1 BTC 結算合約。

2. 如果三天已經過去了，那麼上述條款無效，清算過程也無效，雙方三天后都不能結算和要求付款。

3. 任何一方都可以（也應該）按照本合約的條款以參加者選擇的任何方法支付並且早期關閉此合約，只要這一合約中的兩個參與者同意。

4. 違反上述條款將導致最大的懲罰，資金被鎖在合約中，被支付給作為保真債券資金的對方。

---

For clarity of examples, we use days for HTLCs and block height for RSMCs. In reality, the HTLC should also be defined as a block height (e.g. 3 days is equivalent to 432 blocks).

為了闡明例子，我們在 HTLCs 中使用天數，在 RSMC 使用區塊高度。在現實中，HTLC 也應該被定義為一個區塊高度（例如 3 天相當於 432 區塊）。

---

In effect, one desires to construct a payment which is contingent upon knowledge of R by the recipient within a certain timeframe. After this timeframe, the funds are refunded back to the sender. Similar to RSMCs, these contract terms are programatically enforced on the Bitoin blockchain and do not require trust in the counterparty to adhere to the contract terms, as all violations are penalized via unilaterally enforced fidelity bonds, which are constructed using penalty transactions spending from commitment states. If Bob knows R within three days, then he can redeem the funds by broadcasting a transaction; Alice is unable to withhold the funds in any way, because the script returns as valid when the transaction is spent on the Bitcoin blockchain.

事實上，人們希望建立一個支付，這個支付取決於收件人在一定的時間內對 R 的資訊。在此期限後，該資金退還給寄件者。
類似于 RSMC，這些合約條款是在比特幣區塊鏈上強制性程式設計的，不需要對方服從合約條款的信任，因為所有的違反條款的行為都通過單方面強制網路保真債券資金受到懲罰，承諾交易投入手續費設置處罰。如果 Bob 在三天內知道 R，那麼他就可以廣播交易以贖回資金;Alice 是無法以任何方式截留資金的，因為當比特幣區塊鏈上發生了交易，腳本有效地返回。

---

An HTLC is an additional output in a Commitment Transaction with a unique output script:
```
OP IF
  OP HASH160 <Hash160 (R)> OP EQUALVERIFY
  2 <Alice 2 > <Bob2> OP CHECKMULTISIG

OP ELSE
  2 <Alice 1 > <Bob1> OP CHECKMULTISIG
 
OP ENDIF
```
一個 HTLC 是一個具有獨特的輸出腳本承諾交易的額外的輸出： 
```
OP IF
  OP HASH160 <Hash160 (R)> OP EQUALVERIFY
  2 <Alice 2 > <Bob2> OP CHECKMULTISIG

OP ELSE
  2 <Alice 1 > <Bob1> OP CHECKMULTISIG
 
OP ENDIF
```

---

Conceptually, this script has two possible paths spending from a single HTLC output. The first path (defined in the OP IF) sends funds to Bob if Bob can produce R. The second path is redeemed using a 3-day timelocked refund to Alice. The 3-day timelock is enforced using nLockTime from the spending transaction.

從概念上講，這個腳本從單一的 HTLC 輸出花費有兩種可能的路徑。在第一個路徑（定義為 OP IF）將資金發送給 Bob，如果 Bob 可以產生 R.。第二條路徑是被贖回，使用 3 天
timelocked 退款給 Alice。為期 3 天的 timelock 使用來自於消費交易的 nLockTime 執行。

---

### 4.1 不可撤銷的 HTLC 結構 | Non-revocable HTLC Construction

![](image/figure11.png)

Figure 11: This is a non-functional naive implementation of an HTLC. Only the HTLC path from the Commitment Transaction is displayed. Note that there are two possible spends from an HTLC output. If Bob can produce the preimage R within 3 days and he can redeem path 1. After three days, Alice is able to broadcast path 2. When 3 days have elapsed either is valid. This model, however, doesn’t work with multiple Commitment Transactions.

圖 11：這是一個 HTLC 的非功能性前期執行。只有來自於承諾交易的 HTLC 路徑可以被顯示。注意有兩種可能的來自於 HTLC 輸出花費。如果 Bob 能在 3 天之內生產原像 R，他可以贖回路徑 1.三天后，Alice 能夠廣播路徑 2。當 3 天已過或者是有效的。然而，該模型中，這並不與多個承諾交易工作。

---

If R is produced within 3 days, then Bob can redeem the funds by broadcasting the “Delivery” transaction. A requirement for the “Delivery” transaction to be valid requires R to be included with the transaction. If R is not included, then the “Delivery” transaction is invalid. However, if 3 days have elapsed, the funds can be sent back to Alice by broadcasting transaction “Timeout”. When 3 days have elapsed and R has been disclosed, either transaction may be valid.

如果 R 是在 3 天之內產生的，那麼 Bob 可以通過廣播“交付”交易贖回資金。“交付”交易有效的一個要求是 R 被包含在交易內。若 R 不被包括，則“交付”交易無效。但是，如果 3 天內已過，資金可以通過廣播交易“Timeout”發回給 Alice。3 天后，R 已經被公開，任何交易可能是有效的。

---

It is within both parties individual responsibility to ensure that they can get their transaction into the blockchain in order to ensure the balances are correct. For Bob, in order to receive the funds, he must either broadcast the “Delivery” transaction on the Bitcoin blockchain, or otherwise settle with Alice (while cancelling the HTLC). For Alice, she must broadcast the “Timeout” 3 days from now to receive the refund, or cancel the HTLC entirely with Bob.

這是雙方個人範圍內的責任，以確保他們的交易進入區塊鏈，以保證餘額是正確的。對於 Bob，為了獲得資金，他必須要麼廣播比特幣區塊鏈的“交付”交易，或與 Alice 結算（同時取消 HTLC）。對於 Alice，她必須從即日起 3 天內廣播的“Timeout”交易，以收到退款，或與 Bob 完全取消 HTLC。

---

Yet this kind of simplistic construction has similar problems as an incorrect bidirectional payment channel construction. When an old Commitment Transaction gets broadcast, either party may attempt to steal funds as both paths may be valid after the fact. For example, if R gets disclosed 1 year later, and an incorrect Commitment Transaction gets broadcast, both paths are valid and are redeemable by either party; the contract is not yet enforcible on the blockchain. Closing out the HTLC is absolutely necessary, because in order for Alice to get her refund, she must terminate the contract and receive her refund. Otherwise, when Bob discovers R after 3 days have elapsed, he may be able to steal the funds which should be going to Alice. With uncooperative counterparties it’s not possible to terminate an HTLC without broadcasting it to the bitcoin blockchain as the uncooperative party is unwilling to create a new Commitment Transaction.

然而，這種簡單的結構也有類似於不正確的雙向支付通道結構的問題。當舊的承諾交易被公 布，任何一方都可以試圖竊取資金，因為在此事後，兩個路徑可能是有效的。例如，若 R 被公開 1 年以後，並且不正確的承諾交易被廣播，兩個路徑都有效並且可由任何一方贖回;合約還沒有在區塊鏈上被執行。關閉 HTLC 是絕對必要的，因為 Alice 為了得到退款，她必須終止合約，並接受她的退款。否則，當 Bob3 天后發現 R，他可能能夠竊取應給 Alice 的資金。對於不合作的對方，不可能在沒有把它廣播在區塊鏈時終止 HTLC，因為不合作的一方不願建立新的承諾交易。

---

### 4.2	Off-chain 可撤銷 HTLC | Off-chain Revocable HTLC

To be able to terminate this contract off-chain without a broadcast to the Bitcoin blockchain requires embedding RSMCs in the output, which will have a similar construction to the bidirectional channel.

為了能夠在不廣播到比特幣區塊鏈情況下終止 Off-chain 合約，需要在輸出中嵌入
RSMCs，RSMCs 將與雙向通道有類似結構。

---

![](image/figure12.png)

Figure 12: If Alice broadcasts C2a, then the left half will execute. If Bob broadcasts C2b, then the right half will execute. Either party may broadcast their Commitment transaction at any time. HTLC Timeout is only valid after 3 days. HTLC Executions can only be broadcast if the preimage to the hash R is known. Prior Commitments (and their dependent transactions) are not displayed for brevity.

圖 12：如果 Alice 廣播 C2a，則左半將執行。如果 Bob 廣播 C2b，右半將執行。任何一方都可以在任何時候廣播其交易承諾。 HTLC Timeout 僅在 3 天后生效。只有雜湊 R 的原像是 已知的，HTLC 執行才能被廣播。為了簡潔，先前的承諾（和它們的相關交易）不顯示。

---

Presume Alice and Bob wish to update their balance in the channel at Commitment 1 with a balance of 0.5 to Alice and 0.5 to Bob.

假設 Alice 和 Bob 希望在承諾 1 通道中以 0.5 給 Alice，0.5 給 Bob 方式更新餘額。

---

Alice wishes to send 0.1 to Bob contingent upon knowledge of R within 3 days, after 3 days she wants her money back if Bob does not produce R. 

Alice 希望在 3 天內在已知 R 的資訊的情況下發送 0.1 給 Bob，三天后，如果 Bob 不產生 R，她希望要回她的錢。

---

The new Commitment Transaction will have a full refund of the cur-
rent balance to Alice and Bob (Outputs 0 and 1), with output 2 being the HTLC, which describes the funds in transit. As 0.1 will be encumbered in an HTLC, Alice’s balance is reduced to 0.4 and Bob’s remains the same at 0.5.

新的承諾交易將有一個對於 Alice 和 Bob（輸出 0 和 1）現有的餘額的全額退款，HTLC 中沒有輸出 2，輸出 2 描述了在途資金。 0.1 將受限於 HTLC 中，Alice 的餘額下降到 0.4，Bob 保持不變為 0.5。

---

This new Commitment Transaction (C2a/C2b) will have an HTLC
output with two possible spends. Each spend is different depending on each counterparty’s version of the Commitment Transaction. Similar to the bidirectional payment channel, when one party broadcasts their Commitment, payments to the counterparty will be assumed to be valid and not invalidated. This can occur because when one broadcasts a Commitment Transaction, one is attesting this is the most recent Commitment Transaction. If it is the most recent, then one is also attesting that the HTLC exists and was not invalidated before, so potential payments to one’s counterparty should be valid.

這一新的承諾交易（C2A / C2b 上）將有一個有兩個可能的花費的 HTLC 輸出。每個支出是不同的，根據每個交易對方的承諾交易的版本。類似於雙向支付通道，當一方廣播他們的承諾，給交易對方的支付會被認為是有效的而不是無效的。這可能發生，因為當一方廣播承諾交易，是證明這是最近的承諾交易。如果它是最近的，也證明該 HTLC 存在並且之前未失效，所以給另一方的潛在支付應該是有效的。

---

Note that HTLC transaction names (beginning with the letter H) will begin with the number 1, whose values do not correlate with Commitment Transactions. This is simply the first HTLC transaction. HTLC transactions may persist between Commitment Transactions. Each HTLC has 4 keys per side of the transaction (C2a and C2b) for a total of 8 keys per counterparty.

注意，HTLC 交易名稱（用字母 H 開始）將以數字 1 開始，其值不與承諾交易相關。這僅僅是第一個 HTLC 交易。HTLC 交易在承諾交易之間依然存在。每個 HTLC 在交易的每個側面（C2A 和 C2B）具有 4 個鍵，總計每個對方 8 鍵。

---

The HTLC output in the Commitment Transaction has two sets of keys per counterparty in the output.

在承諾交易的 HTLC 輸出中每個對方有兩組輸出金鑰。

---

For Alice’s Commitment Transaction (C2a), the HTLC output script requires multisig(PAlice2, PBob2) encumbered by disclosure of R, as well as multisig(PAlice1, PBob1) with no encumbering.

Alice 的承諾交易（C2a）中，HTLC 輸出腳本需要通過公開的 R 受阻礙的 multisig（PAlice2， PBob2），以及不受阻礙的 multisig（PAlice1，PBob1）。

---

For Bob’s Commitment Transaction (C2b), the HTLC output script requires multisig(PAlice6, PBob6) encumbered by disclosure of R, as well as multisig(PAlice5, PBob5) with no encumbering.

Bob 的承諾交易（C2b）中，HTLC 輸出腳本需要通過公開的 R 受阻礙的 multisig（PAlice6，PBob6），以及不受阻礙的 multisig（PAlice5，PBob5）。

---

The HTLC output states are different depending upon which Commitment Transaction is broadcast.

該 HTLC 輸出狀態是根據哪個承諾交易被廣播的。

---

#### 4.2.1 當寄件者廣播的承諾交易 HTLC | HTLC when the Sender Broadcasts the Commitment Transaction

For the sender (Alice), the “Delivery” transaction is sent as an HTLC Execution Delivery transaction (HED1a), which is not encumbered in an RSMC. It assumes that this HTLC has never been terminated off-chain, as Alice is attesting that the broadcasted Commitment Transaction is the most recent. If Bob can produce the preimage R, he will be able to redeem funds from the HTLC after the Commitment Transaction is broadcast on the blockchain. This transaction consumes multisig(PAlice2, PBob2) if Alice broadcasts her Commitment C2a. Only Bob can broadcast HED1a since only Alice gave her signature for HED1a to Bob.

對於寄件者（Alice），“交付”交易作為 HTLC 執行交付交易（HED1a）被發送，其不受阻於 RSMC。假定該 HTLC 從未被 Off-chain 終止，因為 Alice 證明廣播的承諾交易是最近的。如果 Bob 可以產生原像 R，他將能夠在該承諾交易在區塊鏈上廣播之後贖回資金。如果 Alice 廣播她的承諾 C2a，本次交易需要 multisig（PAlice2，PBob2）。只有 Alice 給 Bob 她的 HED1a 簽名，Bob 才可以廣播 HED1a。

---

However, if 3 days have elapsed since forming the HTLC, then Alice will be able broadcast a “Timeout” transaction, the HTLC Timeout transaction (HT1a). This transaction is an RSMC. It consumes the output multisig(PAlice1, PBob1) without requiring disclosure of R if Alice broadcasts C2a. This transaction cannot enter into the blockchain until 3 days have elapsed. The output for this transaction is an RSMC with multisig(PAlice3, PBob3) with relative maturity of 1000 blocks, and multisig(PAlice4, PBob4) with no requirement for confirmation maturity. Only Alice can broadcast HT1a since only Bob gave his signature for HT1a to Alice.

但是，如果形成 HTLC 三天已經過去了，Alice 就可以廣播“Timeout”交易了，HTLC Timeout
交易（HT1a）。這項交易是一個 RSMC。它在 Alice 廣播 C2a 的情況下需要輸出 multisig （PAlice1，PBob1），而無需披露 R。本次交易無法進入區塊鏈直到 3 天過後。此交易的輸出是一個有 1000 個區塊相對成熟的 multisig（PAlice3，PBob3）的 RSMC，和不需要區塊確認成熟的 multisig（PAlice4，PBob4）。只有 Bob 給 Alice 他 HT1a 的簽名，Alice 才可以廣播 HT1a。

---

After HT1a enters into the blockchain and 1000 block confirmations occur, an HTLC Timeout Revocable Delivery transaction (HTRD1a) may be broadcast by Alice which consumes multisig(PAlice3, PBob3). Only Alice can broadcast HTRD1a 1000 blocks after HT1a is broadcast since only Bob gave his signature for HTRD1a to Alice. This transaction can be revocable when another transaction supersedes HTRD1a using multisig(PAlice4, PBob4) which does not have any block maturity requirements.

HT1A 進入區塊鏈並且 1000 次確認完成後，一個 HTLC Timeout 撤銷交付交易（HTRD1a）可以由 Alice 通過消耗 multisig（PAlice3，PBob3）廣播。只有 Bob 給 Alice 他 HTRD1a 的簽名，Alice 可以在廣播 HT1a1000 區塊後廣播 HTRD1a。本次交易可以撤銷，當另一個使用 multisig（PAlice4，PBob4）的交易取代 HTRD1a，它沒有對任何區塊的成熟度要求。

---

#### 4.2.2 接收者廣播承諾交易時的 HTLC | HTLC when the Receiver Broadcasts the Commitment Transaction

For the potential receiver (Bob), the “Timeout” of receipt is refunded as an HTLC Timeout Delivery transaction (HTD1b). This transaction directly refunds the funds to the original sender (Alice) and is not encumbered in an RSMC. It assumes that this HTLC has never been terminated off-chain, as Bob is attesting that the broadcasted Commitment Transaction (C2b) is the most recent. If 3 days have elapsed, Alice can broadcast HTD1b and take the refund. This transaction consumes multisig(PAlice5, PAlice5) if Bob broadcasts C2b. Only Alice can broadcast HTD1b since Bob gave his signature for HTD1b to Alice.

對於潛在的接收者（Bob），收到的“Timeout”作為 HTLC Timeout 交付交易（HTD1b） 被退還。本次交易直接返還資金給原始寄件者（Alice），並不受 RSMC 的阻礙。假定該 HTLC 從未被 Off-chain 終止，因為 Bob 證明廣播的承諾交易（C2b）是最新的。如果 3 天已經過 去，Alice 可以廣播 HTD1b 並拿到退款。如果 Bob 廣播 C2b，本次交易需要 multisig（PAlice5， PAlice5）。只有 Alice 可以廣播 HTD1b，因為 Bob 給了 Alice 他 HTD1b 交易的簽名。

---

However, if HTD1b is not broadcast (3 days have not elapsed) and Bob knows the preimage R, then Bob will be able to broadcast the HTLC Execution transaction (HE1b) if he can produce R. This transaction is an RSMC. It consumes the output multisig(PAlice6, PBob6) and requires disclosure of R if Bob broadcasts C2b. The output for this transaction is an RSMC with multisig(PAlice7, PBob7) with relative maturity of 1000 blocks, and multisig(PAlice8, PBob8) which does not have any block maturity requirements. Only Bob can broadcast HE1b since only Alice gave her signature for HE1b to Bob.

但是，如果 HTD1b 沒有被廣播（沒有經過 3 天時間）並且 Bob 知道原像 R，如果他能產生 R.，則 Bob 將能夠廣播 HTLC 執行交易（HE1b）。這項交易是一個 RSMC。如果 Bob 廣播 C2b，它需要輸出 multisig（PAlice6，PBob6），並要求披露 R。此交易的輸出是一個有 1000 個區塊相對成熟的 multisig（PAlice7，PBob7）的 RSMC，和不需要區塊確認成熟的 multisig
（PAlice8，PBob8）。只有 Alice 給 Bob 她 HT1a 的簽名，Bob 才可以廣播 HT1a。

---

After HE1b enters into the blockchain and 1000 block confirmations occur, an HTLC Execution Revocable Delivery transaction (HERD1b) may be broadcast by Bob which consumes multisig(PAlice7, PBob7). Only Bob can broadcast HERD1b 1000 blocks after HE1b is broadcast since only Alice gave her signature for HERD1b to Bob. This transaction can be revocable when another transaction supersedes HERD1b using multisig(PAlice8, PBob8) which does not have any block maturity requirements.

HT1A 進入區塊鏈並且 1000 次確認完成後，一個 HTLC Timeout 撤銷交付交易（HERD1b）可以由 Bob 通過消耗 multisig（PAlice7，PBob7）廣播。只有 Alice 給 Bob 他 HERD1b 的簽名，Bob 可以在廣播 HE1b 1000 區塊後廣播 HERD1b。本次交易可以撤銷，當另一個使用 multisig（PAlice8，PBob8）的交易取代 HERD1b，它沒有對任何區塊的成熟度要求。

---

### 4.3	HTLC Off-chain 終止 | 4.3	HTLC Off-chain Termination

After an HTLC is constructed, to terminate an HTLC off-chain requires both parties to agree on the state of the channel. If the recipient can prove knowledge of R to the counterparty, the recipient is proving that they are able to immediately close out the channel on the Bitcoin blockchain and receive the funds. At this point, if both parties wish to keep the channel open, they should terminate the HTLC off-chain and create a new Commitment Transaction reflecting the new balance.

HTLC 構造之後，為了終止 HTLC Off-chain 需要雙方同意通道的狀態。如果收件人可以向對方證明 R 的資訊，證明他們能夠立即關閉比特幣區塊鏈上的通道並且接收資金。在這一點上，如果雙方都希望保持通道打開，就應終止 HTLC Off-chain，並創建一個新的承諾交易反應新的餘額。

---

![](image/figure13.png)

Figure 13: Since Bob proved to Alice he knows R by telling Alice R, Alice is willing to update the balance with a new Commitment Transaction. The payout will be the same whether C2 or C3 is broadcast at this time.

圖 13：由於 Bob 向 Alice 證明，以告訴 Alice R 的有關資訊來告訴 Alice，Alice 願意用新的承諾交易更新餘額。此時不管廣播 C2 或 C3，支付將是相同的。

---

Similarly, if the recipient is not able to prove knowledge of R by disclosing R, both parties should agree to terminate the HTLC and create a new Commitment Transaction with the balance in the HTLC refunded to the sender.

同樣，如果收件人不能夠通過公開 R 來證明 R 的資訊，雙方應同意終止 HTLC 並創建一個新的承諾交易， HTLC 中的餘額退還給寄件者。

---

If the counterparties cannot come to an agreement or become otherwise unresponsive, they should close out the channel by broadcasting the necessary channel transactions on the Bitcoin blockchain.

如果交易對方不能達成協議或不回應，他們應該通過在比特幣區塊鏈廣播必需的通道交易來關閉通道。

---

However, if they are cooperative, they can do so by first generating a new Commitment Transaction with the new balances, then invalidate the prior Commitment by exchanging Breach Remedy transactions (BR2a/BR2b). Additionally, if they are terminating a particular HTLC, they should also exchange some of their own private keys used in the HTLC transactions.

但是，如果他們合作，他們可以通過首先生成具有新的餘額的承諾交易，然後通過交換違約 補救交易（BR2a / BR2b）使先前承諾失效。此外，如果他們終止特定的 HTLC，也要交換一些在 HTLC 交易中使用的自己的私密金鑰。

---

For example, Alice wishes to terminate the HTLC, Alice will disclose KAlice1 and KAlice4 to Bob. Correspondingly if Bob wishes to terminate the HTLC, Bob will disclose KBob6 and KBob8 to Alice. After the private keys are disclosed to the counterparty, if Alice broadcasts C2a, Bob will be able to take all the funds from the HTLC immediately. If Bob broadcasts C2b, Alice will be able to take all funds from the HTLC immediately. Note that when an HTLC is terminated, the older Commitment Transaction must be revoked as well.

例如，Alice 希望終止 HTLC，Alice 將披露 KAlice1 和 KAlice4 給 Bob。相應地，如果 Bob 希望終止 HTLC，Bob 將披露 KBob6 和 KBob8 給 Alice。私密金鑰透露給對方之後，如果 Alice 廣播 C2A，Bob 就能夠立即從 HTLC 拿走一切資金。如果 Bob 廣播 C2b，Alice 將能夠立即拿走 HTLC 上的一切資金。需要注意的是，當一個 HTLC 終止時，較舊的承諾交易必須也被撤銷。

---

![](image/figure14.png)

Figure 14: A fully revoked Commitment Transaction and terminated HTLC. If either party broadcasts Commitment 2, they will lose all their money to the counterparty. Other commitments (e.g. if Commitment 3 is the current Commitment) are not displayed for brevity.

圖 14：一個完全撤銷的承諾交易及終止的 HTLC。如果任何一方廣播承諾 2，他們將失去所有的錢，交給對方。簡潔為了，其他承諾（例如，如果承諾 3 是當前承諾）不顯示。

---

Since both parties are able to prove the current state to each other, they can come to agreement on the current balance inside the channel. Since they may broadcast the current state on the blockchain, they are able to come to agreement on netting out and terminating the HTLC with a new Commitment Transaction.

因為雙方都能夠彼此證明當前狀態，他們可以就現有通道中的餘額達成一致意見。因為它們可以在區塊鏈上廣播目前的狀態，他們能就用一個新的承諾交易剔除並終止 HTLC 達成一致意見。

---

### 4.4 HTLC 形成和封閉令 | HTLC Formation and Closing Order

To create a new HTLC, it is the same process as creating a new Commitment Transaction, except the signatures for the HTLC are exchanged before the new Commitment Transaction’s signatures.

要創建一個新的 HTLC，這與創建一個新的承諾交易有相同的過程，除了 HTLC 的簽名在新的承諾交易簽名交換之前被交換。

---

To close out an HTLC, the process is as follows (from C2 to C3):

1.	Alice signs and sends her signature for RD3b and C3b. At this point Bob can elect to broadcast C3b or C2b (with the HTLC) with the same payout. Bob is willing after receiving C3b to close out C2b.
2.	Bob signs and sends his signature for RD3a and C3a, as well as his private keys used for Commitment 2 and the HTLC being terminated; he sends Alice KBobRSMC2, KBob5, and KBob8. At this point Bob should only broadcast C3b and should not broadcast C2b as he will lose all his money if he does so. Bob has fully revoked C2b and the HTLC. Alice is willing after receiving C3a to close out C2b.
3.	Alice signs and sends her signature for RD3b and C3b, as well as her private keys used for Commitment 2 and the HTLC being terminated; she sends Bob KAliceRSMC2, KBob1, and KBob4. At this point neither party should broadcast Commitment 2, if they do so, their funds will be going to the counterparty. The old Commitment and old HTLC are now revoked and fully terminated. Only the new Commitment 3 remains, which does not have an HTLC.

關閉一個 HTLC，該過程如下（從 C2 至 C3）：

1. Alice 簽署並發送她 RD3b 和 C3b 的簽名。此時 Bob 可以選擇廣播的 C3b 或 C2b（與 HTLC），其具有相同的支出。Bob 願意接收 C3b 並關閉 C2b。

2. Bob 簽署並發送他 RD3a 及 C3a 的簽名，以及他用於承諾 2 的私人金鑰並且 HTLC 被終止; 他發送 KBobRSMC2，KBob5 和 KBob8 給 Alice。在這一點上 Bob 只能廣播的 C3b，不應公布 C2b，如果他這樣做他將失去他所有的錢。 Bob 已經完全撤銷 C2b 和 HTLC。 Alice 願意接受 C3a 並關閉 C2b。

3. Alice 簽署並發送她 RD3b 和 C3b 的簽名，以及她用於承諾 2 的私人金鑰並且 HTLC 被終止;她發送 KAliceRSMC2，KBob1 和 KBob4 給 Bob。此時，任何一方應廣播承諾 2，如果他們這樣做，他們的資金將流向對方。舊的承諾和舊的 HTLC 現已撤銷並完全終止。只有沒有 HTLC 的新承諾 3 遺留下來。

---

When the HTLC has been closed, the funds are updated so that the present balance in the channel is what would occur had the HTLC contract been completed and broadcast on the blockchain. Instead, both parties elect to do off-chain novation and update their payments inside the channel.

當 HTLC 已被關閉，資金被更新，使得在通道內現有的餘額是在完成並在區塊鏈上廣播HTLC 合約會發生的。相反，雙方都選擇 Off-chain 更新並在通道內更新自己的付款。

---

It is absolutely necessary for both parties to complete off-chain novation within their designated time window. For the receiver (Bob), he must know R and update his balance with Alice within 3 days (or whatever time was selected), else Alice will be able to redeem it within 3 days. For Alice, very soon after her timeout becomes valid, she must novate or broadcast the HTLC Timeout transaction. She must also novate or broadcast the HTLC Timeout Revocable Delivery transaction as soon as it becomes valid. If the counterparty is unwilling to novate or is stalling, then one must broadcast the current channel state, including HTLC transactions) onto the Bitcoin blockchain.

雙方當事人在其指定的時間範圍內完成 Off-chain 更新是絕對必要的。對於接收者（Bob），他必須知道 R 和與 Alice 之間的 3 天之內的餘額（或任何被選中的時間），否則 Alice 將能夠在 3 天內贖回。對於 Alice，她的 Timeout 有效後不久，她必須更替或廣播的 HTLC Timeout 交易。她還必須更替或廣播 HTLC Timeout 撤銷交付交易，一旦它成為有效的。如果對方不

---

The amount of time flexibility with these offers to novate are dependent upon one’s contingent dependencies on the hashlock R. If one establishes a contract that the HTLC must be resolved within 1 day, then if the transaction times out Alice must resolve it by day 4 (3 days plus 1), else Alice risks losing funds.

願意更替或延遲，那麼就必須廣播當前通道狀態(包括 HTLC 交易）到比特幣區塊鏈。時間的靈活性與這些更替的提供取決於一方對 hashlock R 偶然依賴性.，一方如果發佈一個合約，HTLC 必須在 1 天之內解決，那麼如果交易超時，Alice 必須在 4 天內解決它（3 天加 1 天），否則 Alice 可能失去資金。

---

## 5 金鑰儲存 | Key Storage

Keys are generated using BIP 0032 Hierarchical Deterministic Wallets[17]. Keys are pre-generated by both parties. Keys are generated in a merkle tree and are very deep within the tree. For instance, Alice pre-generates one million keys, each key being a child of the previous key. Alice allocates which keys to use according to some deterministic manner. For example, she starts with the child deepest in the tree to generate many sub-keys for day 1. This key is used as a master key for all keys generated on day 1. She gives Bob the address she wishes to use for the next transaction, and discloses the private key to Bob when it becomes invalidated. When Alice discloses to Bob all private keys derived from the day 1 master key and does not wish to continue using that master key, she can disclose the day 1 master key to Bob. At this point, Bob does not need to store all the keys derived from the day 1 master key. Bob does the same for Alice and gives her his day 1 key.

使用 BIP 0032 分層確定性錢包[17]生成金鑰。金鑰是通過雙方預先生成的。在 MERKLE 樹生成金鑰，並且非常深的隱藏在樹內。例如，Alice 預生成百萬個金鑰，每個金鑰是前一個金鑰的子金鑰。Alice 根據一些確定的方式分配使用哪個金鑰。例如，她第 1 天開始用樹最底層的子金鑰來生成更多的金鑰。這一金鑰是在第一天生成的所有金鑰的主金鑰。她給 Bob 她希望使用的下一個交易地址，並在私密金鑰變為無效時公開給 Bob。當 Alice 向 Bob 公開了由主金鑰派生的所有私密金鑰，並且不希望繼續使用該主金鑰時，她可以把每天的主金鑰透露給 Bob。在這一點上，Bob 不需要儲存所有由主金鑰產生的金鑰。Bob 做同樣的事，給 Alice 他第一天的主金鑰。

---

When all Day 2 private keys have been exchanged, for example by day 5, Alice discloses her Day 2 key. Bob is able to generate the Day 1 key from the Day 2 key, as the Day 1 key is a child of the Day 2 key as well.

當所有的第 2 天的私密金鑰交換完成，例如在第 5 天之前，Alice 廣播了她第 2 天的主金鑰。Bob 是能夠從第一天的主金鑰產生第 2 天的主金鑰，因為第 2 天主金鑰也是第一天的主金鑰的子金鑰。

---

If a counterparty broadcasts the wrong Commitment Transaction, which private key to use in a transaction to recover funds can either be brute forced, or if both parties agree, they can use the sequence id number when creating the transaction to identify which sets of keys are used.

如果對方廣播了錯誤的承諾交易，在交易中回收資金使用的私有金鑰既可以被強制執行，或者如果雙方同意，他們可以在創建交易時使用 id 序列數位來確定哪些金鑰可以被使用。

---

This enables participants in a channel to have prior output states (transactions) invalidated by both parties without using much data at all. By disclosing private keys pre-arranged in a merkle-tree, it is possible to invalidate millions of old transactions with only a few kilobytes of data per channel. Core channels in the Lightning Network can conduct billions of transactions without a need for significant storage costs.

這使得通道參與雙方能夠使之前的輸出狀態（交易）失效，並且不使用大量的資料。通過公開一個 MERKLE 樹中預先安排的私密金鑰，僅僅使用每個通道中幾個KB的資料來使百萬舊記錄無效是可能的。閃電網路的核心通道可以進行數十億美元的交易，而不需要大量的儲存成本。

---

## 6 雙向通道的區塊鏈交易費 | Blockchain Transaction Fees for Bidirectional Channels

It is possible for each participant to generate different versions of transactions to ascribe blame as to who broadcast the transaction on the blockchain. By having knowledge of who broadcast a transaction and the ability to ascribe blame, a third party service can be used to hold fees in a 2-of-3 multisig escrow. If one wishes to broadcast the transaction chain instead of agreeing to do a Funding Close or replacement with a new Commitment Transaction, one would communicate with the third party and broadcast the chain to the blockchain. If the counterparty refuses the notice from the third party to cooperate, the penalty is rewarded to the non-cooperative party. In most instances, participants may be indifferent to the transaction fees in the event of an uncooperative counterparty.

每個參與者產生不同版本的交易來尋找在區塊鏈上廣播交易的錯誤來源是可能的。通過得知是誰廣播交易並能夠尋找到錯誤的來源，協力廠商服務可以在 2-of-3 multisig 代管用於持有手續費。如果一方希望廣播交易鏈，而不是同意做一個資金關閉或更換新的承諾交易，一方會與協力廠商交流並廣播此交易鏈到區塊鏈。如果對方拒絕來自協力廠商合作的通知，非合作方會受到懲罰。在大多數情況下，參與者在對方不合作的情況下不在乎交易手續費。

---

One should pick counterparties in the channel who will be cooperative, but is not an absolute necessity for the system to function. Note that this does not require trust among the rest of the network, and is only relevant for the comparatively minor transaction fees. The less trusted party may just be the one responsible for transaction fees.

每個人都應該挑選通道中合作的對方，但系統不一定能執行功能。需要注意的是，這並不需要網路的其餘部分之間的信任，而只與較為次要的交易手續費有關。低信任度的一方可能只是一個對交易費負責的一方。

---

The Lightning Network fees will likely be significantly lower than blockchain transaction fees. The fees are largely derived from the time-value of locking up funds for a particular route, as well as paying for the chance of channel close on the blockchain. These should be significantly lower than on-chain transactions, as many transactions on a Lightning Network channel can be settled into one single blockchain transaction. With a sufficiently robust and interconnected network, the fees should asymptotically approach negligibility for many types of transactions. With cheap fees and fast transactions, it will be possible to build scalable micropayments, even amongst high-frequency systems such as Internet of Things applications or per-unit micro-billing.

閃電網路手續費很可能會顯著低於區塊鏈交易手續費。該手續費主要來自於用於一個特定路線的對資金的鎖定，以及支付在區塊鏈中的通道機會。這些應該是比 on-chain 交易低，作為一個閃電網路通道中的交易可落戶到一個單一的區塊鏈交易。一個足夠穩健並且互相連接的網路，對於許多類型的交易，資費應該逐漸地接近忽略不計了。隨著廉價的手續費和快速的交易，將有可能構建可擴展小額支付，甚至在高頻系統，如物聯網應用或 per-unit-micro-billing。

---

## 7 薪酬合約 | Pay to Contract

It is possible construct a cryptographically provable “Delivery Versus Payment” contract, or pay-to-contract[18], as proof of payment. This proof can be established as knowledge of the input R from hash(R) as payment of a certain value. By embedding a clause into the contract between the buyer and seller stating that knowing R is proof of funds sent, the recipient of funds has no incentive to disclose R unless they have certainty that they will receive payment. When the funds eventually get pulled from the buyer by their counterparty in their micropayment channel, R is disclosed as part of that pull of funds. One can design paper legal documents that specify that knowledge or disclosure of R implies fulfillment of payment. The sender can then arrange a cryptographically signed contract with knowledge of inputs for hashes treated as fulfillment of the paper contract before payment occurs.

有可能建立一個加密的可證明的“交付對支付”合約，或者支付到合約[18]，作為付款證明。這個證明可以從雜湊（R）建立輸入 R 的資訊，作為一定的價值的付款。通過在買方和賣方之間嵌入合約的條款來聲稱知道 R 是資金發送的證明，資金的接收方沒有披露 R 的任何動機，除非他們有把握收到付款。當資金最終由買家在他們的對方微支付通道收回，R 披露為資金收回的一部分。一方可以設計出可以將資訊細節化並且披露 R 的紙質法律檔，意味著支付的完成。然後，發送方可以在知道雜湊密碼的輸入資訊的情況下安排加密簽名的合約，被作為交易完成前的紙質合約的完成。

---

## 8 比特幣閃電網路 | The Bitcoin Lightning Network

By having a micropayment channel with contracts encumbered by hashlocks and timelocks, it is possible to clear transactions over a multi-hop payment network using a series of decrementing timelocks without additional central clearinghouses.

通過有小額支付通道，該小額支付通道有由 hashlocks 和 timelocks 作保證的合約，有可能在多跳躍支付網路上用使用一系列遞減 timelocks 無需額外的中央票據交換所的方式來清除交易。

---

Traditionally, financial markets clear transactions by transferring the obligation for delivery at a central point and settle by transferring ownership through this central hub. Bank wire and fund transfer systems (such as ACH and the Visa card network), or equities clearinghouses (such as the DTCC) operate in this manner.

傳統上，金融市場通過在一個中心點轉移義務交付，並通過這個中心樞紐轉讓所有權來清除交易。電匯和資金轉帳系統（如 ACH 和信用卡公司網路），或以這種方式工作的股票清算所（如 DTCC）。

---

As Bitcoin enables programmatic money, it is possible to create transactions without contacting a central clearinghouse. Transactions can execute off-chain with no third party which collects all funds before disbursing it – only transactions with uncooperative channel counterparties become automatically adjudicated on the blockchain.

隨著比特幣使程式設計性質的錢成為可能，無需聯繫中央票據交換所就可以創造交易。交易可以在沒有協力廠商在發放資金之前彙集所有資金的情況下執行 off-chain。只有與不合作通道對方交易時自動在區塊鏈上進行調整。

---

The obligation to deliver funds to an end-recipient is achieved through a process of chained delegation. Each participant along the path assumes the obligation to deliver to a particular recipient. Each participant passes on this obligation to the next participant in the path. The obligation of each subsequent participant along the path, defined in their respective HTLCs, has a shorter time to completion compared to the prior participant. This way each participant is sure that they will be able to claim funds when the obligation is sent along the path.

將資金提供給最終接收者的義務是通過授權鏈的方法實現的。路徑上的每個參與者承擔傳遞給特定的收件人的義務。每個參與者將此義務傳遞給路徑中的下一個參與者。該路徑上的後續參與者的義務定義在各自 HTLCs，比現有參與者需要更短的時間完成。這樣當義務沿所述路徑被發送時，每個參與者能確保他們將能夠要求資金。

---

Bitcoin Transaction Scripting, a form of what some call an implementation of “Smart Contracts”[19], enables systems without trusted custodial clearinghouses or escrow services.

比特幣交易腳本，一些人稱之為“智能合約”[19]的實現，使系統在沒有信任的保管結算所或託管服務的情況下得以生效。

---

### 8.1 遞減的 Timelocks

Presume Alice wishes to send 0.001 BTC to Dave. She locates a route through Bob and Carol. The transfer path would be Alice to Bob to Carol to Dave.

假設 Alice 希望發送 0.001 BTC 給 Dave。她通過 Bob 和 Carol 找到途徑。傳輸路徑將是從Alice 到 Bob 到 Carol 再到 Dave。

---

![](image/figure15.png)

Figure 15: Payment over the Lightning Network using HTLCs.

圖 15：使用 HTLCs 在閃電網路中付款。

---

When Alice sends payment to Dave through Bob and Carol, she requests from Dave hash(R) to use for this payment. Alice then counts the amount of hops until the recipient and uses that as the HTLC expiry. In this case, she sets the HTLC expiry at 3 days. Bob then creates an HTLC with Carol with an expiry of 2 days, and Carol does the same with Dave with an expiry of 1 day. Dave is now free to disclose R to Carol, and both parties will likely agree to immediate settlement via novation with a replacement Commitment Transaction. This then occurs step-by-step back to Alice. Note that this occurs off-chain, and nothing is broadcast to the blockchain when all parties are cooperative.

當 Alice 通過 Bob 和 Carol 支付給 Dave，她要求 Dave 的雜湊（R）來用於此付款。Alice 然後計數跳躍的量，直到收件人用其作為 HTLC 屆滿。在這種情況下，設置 HTLC 屆滿為 3 天。然後，Bob 與 Carol 創建 HTLC，屆滿兩天，而 Carol 與 Dave 創建 HTLC，屆滿 1 天。Dave 現在可以自由地向 Carol 披露 R，雙方可能會同意通過承諾交易更替即時結算。然後就會一步一步的返回到 Alice。注意，這種情況發生在 off-chain 的情況下，若各方是合作的，沒有東西被廣播到區塊鏈上。

---

![](image/figure16.png)

Figure 16: Settlement of HTLC, Alice’s funds get sent to Dave.

圖 16：HTLC 結算，Alice 的資金被發送給 Dave。

---

Decrementing timelocks are used so that all parties along the path know that the disclosure of R will allow the disclosing party to pull funds, since they will at worst be pulling funds after the date whereby they must receive R. If Dave does not produce R within 1 day to Carol, then Carol will be able to close out the HTLC. If Dave broadcasts R after 1 day, then he will not be able to pull funds from Carol. Carol’s responsibility to Bob occurs on day 2, so Carol will never be responsible for payment to Dave without an ability to pull funds from Bob provided that she updates her transaction with Dave via transmission to the blockchain or via novation.

遞減 timelocks 用來讓沿著路徑的各方知道 R 的披露將允許披露方收回資金，因為他們如果在其必須接受 R 之後的日子收回資金，他們會處於最壞的境地。如果 Dave 不能為 Carol 在一天內產生 R，那麼 Carol 就能夠收出 HTLC。如果戴夫 1 天后廣播 R，那麼他將無法從 Carol 收回資金。Carol 對 Bob 的責任發生在第 2 天，所以 Carol 將不再對給 Dave 的支付負責，並且不能從 Bob 那裡收回資金，如果她通過傳輸到區塊鏈或通過承諾交易更替來更新她與 Dave 的交易。

---

In the event that R gets disclosed to the participants halfway through expiry along the path (e.g. day 2), then it is possible for some parties along the path to be enriched. The sender will be able to know R, so due to Pay to Contract, the payment will have been fulfilled even though the receiver did not receive the funds. Therefore, the receiver must never disclose R unless they have received an HTLC from their channel counterparty; they are guaranteed to receive payment from one of their channel counterparties upon disclosure of the preimage.

倘若 R 在沿路徑中途（如：第二天）透露給參與者，則沿著路徑某些方有可能被充實。發送者可以知道 R，所以依照支付給合約，付款已經完成，即使接收者沒有收到這筆資金。因此，接收者必須永遠不要透露 R，除非他們已經從他們的通道交易對方收到了 HTLC;這樣可以保證在披露原像時能從自己的通道對方接收付款。

---

In the event a party outright disconnects, the counterparty will be responsible for broadcasting the current Commitment Transaction state in the channel to the blockchain. Only the failed non-responsive channel state gets closed out on the blockchain, all other channels should continue to update their Commitment Transactions via novation inside the channel. Therefore, counterparty risk for transaction fees are only exposed to direct channel counterparties. If a node along the path decides to become unresponsive, the participants not directly connected to that node suffer only decreased timevalue of their funds by not conducting early settlement before the HTLC close.

倘若一方徹底斷開，交易對方將負責目前的通道中的承諾交易的狀態廣播到區塊鏈上。只有區塊鏈上的失敗的非回應通道狀態被關閉，所有其他通道應繼續通過通道內更替阿來更新自己的承諾交易。因此，對於對方交易手續費風險只能告知直接通道方。如果沿路徑的節點決定變成無回應，沒有直接連接到該節點的參與者只遭受了其資金的時間價值的降低，因為其在 HTLC 關閉之前沒有過早的結算。

---

![](image/figure17.png)

Figure 17: Only the non-responsive channels get broadcast on the blockchain, all others are settled off-chain via novation.

圖 17：只有無回應通道得以在區塊鏈上廣播，所有其他的通過更替進行 off-chain 的結算。

---

### 8.2 付款金額 | Payment Amount

It is preferable to use a small payment per HTLC. One should not use an extremely high payment, in case the payment does not fully route to its destination. If the payment does not reach its destination and one of the participants along the path is uncooperative, it is possible that the sender must wait until the expiry before receiving a refund. Delivery may be lossy, similar to packets on the internet, but the network cannot outright steal funds in transit. Since transactions don’t hit the blockchain with cooperative channel counterparties, it is recommended to use as small of a payment as possible. A tradeoff exists between locking up transaction fees on each hop versus the desire to use as small a transaction amount as possible (the latter of which may incur higher total fees). Smaller transfers with more intermediaries imply a higher percentage paid as Lightning Network fees to the intermediaries.

優先使用每 HTLC 的小額付款。一方不應該使用的極高的支付，以防支付不充分路由到其目的地。如果支付沒有到達其目的地並且沿路徑的參與者之一是不合作的，發送者必須等待，直到接收退款之前的期滿。交付時可能會受損，類似於在互聯網上資料包，但網路不能直接竊取在途資金。由於若通道對方是合作的，交易不會被廣播到區塊鏈上，建議盡可能使用小的支付。在每一次跳躍時鎖定交易手續費與希望用盡可能小的交易金額（後者可能會產生較高的總手續費）之間存在著權衡。有更多的仲介機構的規模較小的轉移意味著更高比例的支付作為閃電網路手續費支付給仲介機構。

### 8.3 清除故障和重新路由 | Clearing Failure and Rerouting

If a transaction fails to reach its final destination, the receiver should send an equal payment to the sender with the same hash, but not disclose R. This will net out the disclosure of the hash for the sender, but may not for the receiver. The receiver, who generated the hash, should discard R and never broadcast it. If one channel along the path cannot be contacted, then the channels may elect to wait until the path expires, which all participants will likely close out the HTLC as unsettled without any payment with a new Commitment Transaction.

如果交易無法到達其網路連接最終目的地，接收應以相同雜湊發送同等數量的支付給發送且從不公開。如果沿著路徑的一個通道無法聯繫，那麼通道可以選擇等待，直到路徑期滿後，所有參與者將有可能關閉不穩定，沒有任何支付的 HTLC，創建一個新的承諾交易。

---

![](image/figure18.png)

Figure 18: Dave creates a path back to Alice after Alice fails to send funds to Dave, because Carol is uncooperative. The input R from hash(R) is never brodcast by Dave, because Carol did not complete her actions. If R was broadcast, Alice will break-even. Dave, who controls R should never broadcast R because he may not receive funds from Carol, he should let the contracts expire. Alice and Bob have the option to net out and close the contract early, as well, in this diagram.

圖 18：Alice 將資金發送給 Dave 失敗後，Dave 創建一條返回 Alice 的路徑，因為 Carol 是不合作的。從雜湊值（R）中產生的輸入 R 永遠不會被 Dave 廣播，因為 Carol 沒有完成她的行動。若 R 廣播，Alice 將盈虧餘額。控制 R 的 Dave 永遠不廣播 R，因為他可能無法從 Carol 獲得資金，他應該讓合約到期。在此圖中，Alice 和 Bob 也可在早期淨出並關閉合約。

---

If the refund route is the same as the payment route, and there are no half-signed contracts whereby one party may be able to steal funds, it is possible to outright cancel the transaction by replacing it with a new Commitment Transaction starting with the most recent node who participated in the HTLC.

如果退回路線與支付途徑是相同的，並且沒有半簽署的合約，在半簽署的合約中一方能夠竊取資金，也能夠通過用新的承諾交易替換它來徹底取消交易，先從最近參加 HTLC 的節點開始。

---

It is also possible to clear out a channel by creating an alternate route path in which payment will occur in the opposite direction (netting out to zero) and/or creating an entirely alternate route for the payment path. This will create a time-value of money for disclosing inputs to hashes on the Lightning Network. Participants may specialize in high connectivity between nodes and offering to offload contract hashlocks from other nodes for a fee. These participants will agree to payments which net out to zero (plus fees), but are loaning bitcoins for a set time period. Most likely, these entities with low demand for channel resources will be end-users who are already connected to multiple well-connected nodes. When an end-user connects to a node, the node may ask the client to lock up their funds for several days to another channel the client has established for a fee. This can be achieved by having the new transactions require a new hash(Y) from input Y in addition to the existing hash which may be generated by any participant, but must disclose Y only after a full circle is established. The new participant has the same responsibility as well as the same timelocks as the old participant being replaced. It is also possible that the one new participant replaces multiple hops.

另外，也可以通過創建備用路由路徑來淨出，其中將發生相反方向的付款（淨出到零）和/或創建用於支付路徑的完全備用路由。這將創造錢在閃電網路上披露輸入雜湊的時間價值。參加者可以專注於連接節點之間的高度聯繫，並且為其他節點清理合約雜湊收取手續費。這些參與者將同意淨輸出為零（加手續費）的支付，但給比特幣設定一個時間段。最有可能的是，這些實體對通道資源成為已經連接到多個良好連接節點的最終用戶的需求較低。當最終用戶連接到一個節點，該節點可以要求用戶端將他們的資金鎖定數天到另一個為了收費已經建立起用戶端的通道。這可以通過使新的交易需要除了現有雜湊，還需要新的來自於輸入 Y 的雜湊（Y）來實現，其可以通過任何參與者生成，但是必須在完全建立後披露 Y。新的參與者與被替換的舊的參與者有相同的職責和 timelocks。一個新的參與者代替多次跳躍是可能的。

---
![](image/figure19.png)

Figure 19: Erin is connected to both Bob and Dave. If Bob wishes to free up his channel with Carol, since that channel is active and very profitable, Bob can offload the payment to Dave via Erin. Since Erin has extra bitcoin available, she will be able to collect some fee for offloading the channel between Bob and Carol as well as between Carol and Dave. The channels between Bob and Carol as well as Carol and Dave are undone and no longer have the HTLC, nor has payment occurred on that path. Payment will occur on the path involving Erin. This is achieved by creating a new payment from Dave to Carol to Bob contingent upon Erin constructing an HTLC. The payment in dashed lines (red) are netted out to zero and settled via a new Commitment Contract.

圖 19：Erin 同時連接到 Bob 和 Dave。如果 Bob 希望釋放他與 Carol 的通道，因為該通道是活動的並且非常有利可圖的，Bob 可以通過 Erin 支付給 Dave。由於愛琳有多餘的可用比特幣，她就可以在 Bob 和 Carol 關閉通道時，同樣在 Carol 和 Dave 之間也可以。Bob 和 Carol， 以及 Carol 和 Dave 之間的通道被撤銷，不再有 HTLC，在這條路徑上也不再有支付。付款會發生在涉及到 Erin 的路徑上。這是通過創建一個新的付款得以實現的，新的付款從 Dave 到 Carol 到 Bob 再到 Erin，Erin 隨即構建一個 HTLC。虛線（紅色）付款是淨出為零，並通過一個新的承諾簽約結算。

---

### 8.4 付款路由 | Payment Routing

It is theoretically possible to build a route map implicitly from observing 2-of-2 multisigs on the blockchain to build a routing table. Note, however, this is not feasible with pay-to-script-hash transaction outputs, which can be resolved out-of-band from the bitcoin protocol via a third party routing service. Building a routing table will become necessary for large operators (e.g. BGP, Cjdns). Eventually, with optimizations, the network will look a lot like the correspondent banking network, or Tier-1 ISPs. Similar to how packets still reach their destination on your home network connection, not all participants need to have a full routing table. The core Tier-1 routes can be online all the time —while nodes at the edges, such as average users, would be connected intermittently.

Node discovery can occur along the edges by pre-selecting and offering partial routes to well-known nodes.

理論上可能建立路由圖，通過觀察區塊鏈上個 2-of-2 multisigs 來建立一個路由表。但是需要注意得是，這對於 pay-to-script-hash 交易輸出是不可行的，可以通過協力廠商路由服務解決來自於 out-of-band 比特幣協定。建立一個路由表對大型運營商（如 BGP，Cjdns）是必要的。最終，優化之後，網路看起來很像代理行網路，或者 Tier-1 ISPs。類似于資料包如何在您的家用網路連接上到達目的地，不是所有的參與者需要有一個完整的路由表。核心 Tier-1 路由可一直線上，而節點會在邊緣，如普通用戶，會被間歇性的連接起來。

節點發現可通過預選，發生在邊緣，並且給知名節點提供部分路徑。

---

### 8.5 手續費 | Fees

Lightning Network fees, which differ from blockchain fees, are paid directly between participants within the channel. The fees pay for the time-value of money for consuming the channel for a determined maximum period of time, and for counterparty risk of non-communication.

閃電網路手續費，與區塊鏈手續費不同，是在通道內的參與者之間直接支付。用於支付確定的最大週期內消費通道的資金的時間價值，而對於不通信的交易對方風險。

---

Counterparty risk for fees only exist with one’s direct channel counterparty. If a node two hops away decides to disconnect and their transaction gets broadcast on the blockchain, one’s direct counterparties should not broadcast on the blockchain, but continue to update via novation with a new Commitment Transaction. See the Decrementing Timelocks entry in the HTLC section for more information about counterparty risk.

手續費的對方風險只在與一方的直接通道對方交易時存在。如果兩次跳躍以外的一個節點決定斷開聯繫並且將其交易廣播在區塊鏈上，一方的直接對方不應廣播在區塊鏈上，而是繼續通過更替更新成為一個新的承諾交易。遞減 Timelocks 進入 HTLC 部分，來獲取有關交易對方風險的更多資訊。

---

The time-value of fees pays for consuming time (e.g. 3 days) and is conceptually equivalent to a gold lease rate without custodial risk; it is the time-value for using up the access to money for a very short duration. Since certain paths may become very profitable in one direction, it is possible for fees to be negative to encourage the channel to be available for those profitable paths.

用於支付消費時間的手續費的時間價值（如 3 天），在概念上等同於沒有保管風險的黃金租賃率;它是在一個非常短的時間內訪問資金的時間價值。因為某些路徑在一個方向上可能變得非常有利可圖，手續費有可能變成負數，以鼓勵通道可用於那些有利可圖的路徑。

---

## 9 風險 | Risks

The primary risks relate to timelock expiration. Additionally, for core nodes and possibly some merchants to be able to route funds, the keys must be held online for lower latency. However, end-users and nodes are able to keep their private keys firewalled off in cold storage.

主要風險涉及到 timelock 到期。此外，對於核心節點和一些能夠路由資金的可能的商家，為達到較低的延遲，鑰匙必須保持線上。然而，最終用戶和節點都能夠在防火牆外持有自己的私密金鑰。

---

### 9.1 不當 Timelocks | Improper Timelocks

Participants must choose timelocks with sufficient amounts of time. If insufficient time is given, it is possible that timelocked transactions believed to be invalid will become valid, enabling coin theft by the counterparty. There is a trade-off between longer timelocks and the time-value of money. When writing wallet and Lightning Network application software, it is necessary to ensure that sufficient time is given and users are able to have their transactions enter into the blockchain when interacting with non-cooperative or malicious channel counterparties.

參賽者必須選擇時間充足的 timelocks。如果不給于充分的時間，被認為是無效 timelocked 交易有可能將成為有效的，可能導致對方盜竊資金。較長的 timelocks 和資金的時間價值之間存在著權衡。當編寫錢包和閃電網路應用軟體時，確保提供其足夠的時間是必要的，並保證用戶在與不合作或惡意的通道對方進行交易時能夠在區塊鏈上廣播其交易。

---

### 9.2 被迫滿期的垃圾郵件 | Forced Expiration Spam

Forced expiration of many transactions may be the greatest systemic risk when using the Lightning Network. If a malicious participant creates many channels and forces them all to expire at once, these may overwhelm block data capacity, forcing expiration and broadcast to the blockchain. The result would be mass spam on the bitcoin network. The spam may delay transactions to the point where other locktimed transactions become valid.

許多交易被迫滿期是使用閃電網路時最大的系統性風險。如果一個惡意的參與者創造了許多通道，迫使他們全都一次性失效，這可能會超過塊資料容量，迫使其過期並廣播在區塊鏈上。其結果將是比特幣網路上充滿海量垃圾郵件。垃圾郵件可能會在某種程度上延遲交易，到達其他 locktimed 交易生效的地步。

---

This may be mitigated by permitting one transaction replacement on all pending transactions. Anti-spam can be used by permitting only one transaction replacement of a higher sequence number by the inverse of an even or odd number. For example, if an odd sequence number was broadcast, permit a replacement to a higher even number only once. Transactions would use the sequence number in an orderly way to replace other transactions. This mitigates the risk assuming honest miners. This attack is extremely high risk, as incorrect broadcast of Commitment Transactions entail a full penalty of all funds in the channel.

這可以通過允許一個交易更換所有未決的交易得到緩解。只允許一個交易更換使用偶數或奇數的倒數的更高順序號才可以使用反垃圾郵件。例如，如果奇數序號被廣播，只允許更換一次到更高的偶數。交易將使用有序的序號，以取代其他交易。這減輕了誠實的礦工承擔的風險。這種攻擊是非常高的風險，因為對承諾交易的不正確廣播會帶來的通道內所有資金全部損失。

---

Additionally, one may attempt to steal HTLC transactions by forcing a timeout transaction to go through when it should not. This can be easily mitigated by having each transfer inside the channel be lower than the total transaction fees used. Since transactions are extremely cheap and do not hit the blockchain with cooperative channel counterparties, large transfers of value can be split into many small transfers. This attempt can only work if the blocks are completely full for a long time. While it is possible to mitigate it using a longer HTLC timeout duration, variable block sizes may become common, which may need mitigations.

此外，人們可能通過強制暫停不應停止的交易試圖竊取 HTLC 交易。如果通道內每一筆交易比所使用的總交易手續費低，則可以減輕這種風險。由於交易是非常便宜的，並且如果與合作通道對方交易則不會廣播在區塊鏈上，價值大的傳輸可以分成許多小的傳輸，這只能在區塊很長一段時間內完全充滿才可以實現。雖然可以使用一個較長的 HTLC Timeout 持續時間來減輕它，可變區塊大小可能變得普遍，這可能需要緩解。

---

If this type of transaction becomes the dominant form of transactions which are included on the blockchain, it may become necessary to increase the block size and run a variable blocksize structure and timestop flags as described in the section below. This can create sufficient penalties and disincentives to be highly unprofitable and unsuccessful for attackers, as attackers lose all their funds from broadcasting the wrong transaction, to the point where it will never occur.

一個可變大小的區塊結構和如下面的部分中描述的 timestop 標誌。這可能會造成足夠多的處罰，並不激勵高度不獲利和不成功的攻擊，因為攻擊者失去了他們所有的資金，由於廣播了錯誤的交易，以致再也不會發生的地步。

---

### 9.3 通過分裂盜竊資金 | Coin Theft via Cracking

As parties must be online and using private keys to sign, there is a possibility that, if the computer where the private keys are stored is compromised, coins will be stolen by the attacker. While there may be methods to mitigate the threat for the sender and the receiver, the intermediary nodes must be online and will likely be processing the transaction automatically. For this reason, the intermediary nodes will be at risk and should not be holding a substantial amount of money in this “hot wallet.” Intermediary nodes which have better security will likely be able to out-compete others in the long run and be able to conduct greater transaction volume due to lower fees. Historically, one of the largest component of fees and interest in the financial system are from various forms of counterparty risk – in Bitcoin it is possible that the largest component in fees will be derived from security risk premiums.

各方必須線上，並使用私密金鑰簽署，還有可能，如果其中儲存私密金鑰的電腦被破壞，資金將被攻擊者竊取。雖然可能有方法來減輕對發送者和接收者的威脅，中間節點必須線上，並可能會自動處理交易。出於這個原因，中間節點將處於危險之中，不應該在“熱錢包”中持有如此大量金錢。從長遠來看，具有更好的安全性的中間節點將可能超過其他的節點，並且由於較低的手續費，將可能處理更大量的交易。從歷史上看，手續費的最大的組成部分和金融體系的利息來自於各種形式的交易對方風險-在比特幣中手續費的最大組成部分很可能從安全風險溢價得到。

---

A Funding Transaction may have multiple outputs with multiple Commitment Transactions, with the Funding Transaction key and some Commitment Transactions keys stored offline. It is possible to create an equivalent of a “Checking Account” and “Savings Account” by moving funds between outputs from a Funding Transaction, with the “Savings Account” stored offline and requiring additional signatures from security services.

資金交易可能有多路輸出與多個承諾交易，線下儲存著資金交易金鑰和承諾交易金鑰。通過從資金交易中移動輸出之間的資金來創造 “檢查帳戶”和“儲蓄帳戶”的等價物是可能的，線下儲存“儲蓄帳戶”，並要求安全服務的其他特徵。

---

### 9.4 資料丟失 | Data Loss

When one party loses data, it is possible for the counterparty to steal funds. This can be mitigated by having a third party data storage service where encrypted data gets sent to this third party service which the party cannot decrypt. Additionally, one should choose channel counterparties who are responsible and willing to provide the current state, with some periodic tests of honesty.

當一方資料丟失，對方可能竊取資金。這可以通過一個協力廠商資料儲存服務得到緩解，其中的加密資料被發送到一方不能解密的協力廠商服務。此外，人們應該選擇負責的，並願意提供當前狀態的通道對方，定期測試其誠實度。

---

### 9.5 忘記及時廣播交易 | Forgetting to Broadcast the Transaction in Time

If one does not broadcast a transaction at the correct time, the counterparty may steal funds. This can be mitigated by having a designated third party to send funds. An output fee can be added to create an incentive for this third party to watch the network. Further, this can also be mitigated by implementing OP CHECKSEQUENCEVERIFY.

如果一方沒有在正確的時間廣播交易，交易對方可能會盜取資金。這可以通過由指定的第三方發送資金來緩解。可以增加輸出費來創造一個激勵協力廠商監控網路。此外，這也可以通過實施 OP CHECKSEQUENCEVERIFY 減輕。

---

### 9.6 無法做出必要的 Soft-Forks | Inability to Make Necessary Soft-Forks

Changes are necessary to bitcoin, such as the malleability soft-fork. Additionally, if this system becomes popular, it will be necessary for the system to securely transact with many users and some kind of structure like a blockheight timestop will be desirable. This system assumes such changes to enable Lightning Network to exist entirely, as well as soft-forks ensuring the security is robust against attackers will occur. While the system may continue to operate with only some time lock and malleability soft-forks, there will be necessary soft-forks regarding systemic risks. Without proper community foresight, an inability to establish a timestop or similar function will allow systemic attacks to take place and may not be recognized as imperative until an attack actually occurs.

比特幣必須改變，如延展性的 Soft-Forks。此外，如果此系統變得流行，系統安全地辦理與許多使用者的交易是必要的，並且期待某種像區塊高度 timestop 的結構。這個系統假定這樣的改變使閃電網路完全存在，並且 Soft-Forks 確保安全性是可以抵抗攻擊的發生的。而該系統可能繼續在只有一些時間鎖和延展性的 Soft-Forks 的情況下執行，關於系統性風險 Soft-Forks 是必要的。如果沒有適當的遠見，沒有建立一個 timestop 或相似功能的能力，系統性攻擊可能會發生，並且直到攻擊實際發生才可被認定為當務之急。

### 9.7 勾結礦工攻擊 | Colluding Miner Attacks

Miners may elect to refuse to enter in particular transactions (e.g. Breach Remedy transactions) in order to assist in timeout coin theft. An attacker can pay off all miners to refuse to include certain transactions in their mempool and blocks. The miners can identify their own blocks in an attempt to prove their behavior to the paying attacker.

礦工可以選擇拒絕進入特定的交易（如違約補救交易），以協助 Timeout 資金盜賊。攻擊者 可以收買所有礦工拒絕將某些交易包含在自己的交易池或者區塊當中。礦工們可以識別自己的區塊，試圖向付款攻擊者證明自己的行為。 

---

This can be mitigated by encouraging miners to avoid identifying their own blocks. Further, it should be expected that this kind of payment to miners is malicious activity and the contract is unenforcible. Miners may then take payment and surreptitiously mine a block without identifying the block to the attacker. Since the attacker is paying for this, they will quickly run out of money by losing the fee to the miner, as well as losing all their money in the channel. This attack is unlikely and fairly unattractive as it is far too difficult and requires a high degree of collusion with extreme risk. 

這可以通過鼓勵礦工避免識別自己的區塊來緩解。此外，這種付款給礦工的行為是惡意活動，並且合約是不可執行的。那麼礦工可能拿走支付並且暗中將交易放在自己的區塊中，不將該塊確定給攻擊者。由於攻擊者為此支付，他們將很快失去所有的錢，因為將錢支付給礦工，並且用完通道中所有的錢。這種攻擊是不可能的，並且不具吸引力，因為它實在太困難並且需要具有高度風險的高度勾結。

The risk model of this attack occurirng is similar to that of miners colluding to do reorg attacks: Extremely unlikely with many uncoordinated
miners.

這種攻擊發生的風險模型類似于礦工串通進行整編攻擊：極不可能有很多不協調的礦工。

## 10 區塊大小增加與共識 | Block Size Increases and Consensus

If we presume that a decentralized payment network exists and one user will make 3 blockchain transactions per year on average, Bitcoin will be able to support over 35 million users with 1MB blocks in ideal circumstances (assuming 2000 transactions/MB, or 500 bytes/Tx). This is quite limited, and an increase of the block size may be necessary to support everyone in the world using Bitcoin. A simple increase of the block size would be a hard fork, meaning all nodes will need to update their wallets if they wish to participate in the network with the larger blocks.

如果我們假定一個分散的支付網路存在，一個使用者平均每年將進行 3 筆區塊鏈交易，在理想的情況下，比特幣就能夠支持超過 3500 萬用戶的 1MB 區塊（假設 2000 交易/ MB 或 500 Byte/ TX ）的交易。這是相當有限的，並且增加區塊大小以支持在比特幣世界交易的每個人可能是必要的。區塊大小的簡單增加將是一個 hard fork，這意味著所有的節點都需要更新自己的錢包，如果他們希望參與到具有較大區塊的網路中。

---

While it may appear as though this system will mitigate the block size increases in the short term, if it achieves global scale, it will necessitate a block size increase in the long term. Creating a credible tool to help prevent blockchain spam designed to encourage transactions to timeout becomes imperative.

雖然可能會出現就好像此系統將在短期內緩和塊大小的增加的狀況，如果它達到全球範圍，在長期內就有必要增加區塊的大小。創建一個可靠的工具，以幫助防止區塊鏈垃圾郵件旨在鼓勵過期的交易成為當務之急。

---

To mitigate timelock spam vulnerabilities, non-miner and miners’ consensus rules may also differ if the miners’ consensus rules are more restrictive. Non-miners may accept blocks over 1MB, while miners may have different soft-caps on block sizes. If a block size is above that cap, then that is viewed as an invalid block by other miners, but not by non-miners. The miners will only build the chain on blocks which are valid according to the agreed-upon soft-cap. This permits miners to agree on raising the block size limit without requiring frequent hard-forks from clients, so long as the amount raised by miners does not go over the clients’ hard limit. This mitigates the risk of mass expiry of transactions at once. All transactions which are not redeemed via Exercise Settlement (ES) may have a very high fee attached, and miners may use a consensus rule whereby those transactions are exempted from the soft-cap, making it very likely the correct transactions will enter the blockchain.

為了減輕 timelock 垃圾郵件的漏洞，非礦工和礦工的共識規則也會不同，如果礦工的共識規則更為嚴格。非礦工可以接受區塊大小超過 1MB，而礦工可能會對區塊大小有不同的soft-cap。如果一個區塊的大小超過該 cap，則將被其他礦工被視為無效區塊，非礦工不會這樣認為。礦工們將只能按照商定的 soft-cap 在區塊上建立有效的鏈。這使得礦工們同意提高區塊大小的限制，而不需要頻繁來自用戶端的 hard-forks，只要礦工提出的金額不超過客戶的硬性限制。這減輕了交易的大規模一次性到期的風險。所有未通過結算（ES）贖回的交易可能有非常高的附加手續費，礦工可以使用共識規則，規定一些交易從 soft-cap 中免除，使得很可能正確的交易將進入區塊鏈。

---

When transactions are viewed as circuits and contracts instead of transaction packets, the consensus risks can be measured by the amount of time available to cover the UTXO set controlled by hostile parties. In effect, the upper bound of the UTXO size is determined by transaction fees and the standard minimum transaction output value. If the bitcoin miners have a deterministic mempool which prioritizes transactions respecting a “weak” local time order of transactions, it could become extremely unprofitable and unlikely for an attack to succeed. Any transaction spam time attack by broadcasting the incorrect Commitment Transaction is extremely high risk for the attacker, as it requires an immense amount of bitcoin and all funds committed in those transactions will be lost if the attacker fails.

當交易被視為電路和合約，而不是交易資料包，共識風險可以通過由敵對方控制的支付 UTXO 的可用時間的數量計量。事實上，UTXO 大小的上界是由交易手續費和標準最低交易輸出來確定的。如果比特幣礦工有一個確定的記憶體池，其優先交易考慮“弱”秩序的當地時間的交易，它可能變得極其不受益或者攻擊極不可能成功。通過廣播不正確承諾交易的任何交易時間的垃圾郵件的攻擊對攻擊者來說是非常高風險的，因為它需要比特幣的數額巨大，如果攻擊失敗，用於這些交易的所有資金都將丟失。

---

## 11 用例 | Use case

In addition to helping bitcoin scale, there are many uses for transactions on the Lightning Network:
* Instant Transactions. Using Lightning, Bitcoin transactions are now nearly instant with any party. It is possible to pay for a cup of coffee with direct non-revocable payment in milliseconds to seconds.
* Exchange Arbitrage. There is presently incentive to hold funds on exchanges to be ready for large market moves due to 3-6 block confirmation times. It is possible for the exchange to participate in this network and for clients to move their funds on and off the exchange for orders nearly instantly. If the exchange does not have deep market depth and commits to only permitting limit orders close to the top of the order book, then the risk of coin theft becomes much lower. The exchange, in effect, would no longer have any need for a cold storage wallet. This may substantially reduce thefts and the need for trusted third party custodians.
* Micropayments. Bitcoin blockchain fees are far too high to accept micropayments, especially with the smallest of values. With this system, near-instant micropayments using Bitcoin without a 3rd party custodian would be possible. It would enable, for example, paying per-megabyte for internet service or per-article to read a newspaper.
* Financial Smart Contracts and Escrow. Financial contracts are especially time-sensitive and have higher demands on blockchain computation. By moving the overwhelming majority of trustless transactions off-chain, it is possible to have highly complex transaction contract terms without ever hitting the blockchain.

***未完成：~~Cross-Chain Payments. So long as there are similar hash-functions across chains, it’s possible for transactions to be routed over multiple chains with different consensus rules. The sender does not have to trust or even know about the other chains – even the destination chain. Simiarly, the receiver does not have to know anything about the sender’s chain or any other chain. All the receiver cares about is a conditional payment upon knowledge of a secret on their chain. Payment can be routed by participants in both chains in the hop. E.g. Alice is on Bitcoin, Bob is on both Bitcoin and X-Coin and Carol is on a hypothetical X-Coin, Alice can pay Carol without understanding the X-Coin consensus rules.~~***


除了説明比特幣規模，對閃電網路上的交易也是很有用的：

* 即時交易。使用閃電網路，比特幣的交易現在幾乎與任何一方即時同步。以毫秒為單位直 接為一杯咖啡支付不可撤銷的付款是可能的。
 

* 外匯套利。目前有留住交換資金並為大市場向 3-6 次區塊確認發展的激勵。這一激勵加入網路是可能的，並可立刻為客戶將他們的資金移進或者移出此交換。如果該交換不具有深入的市場深度並且承諾只允許接近訂單數量的上限，資金被盜的風險就低得多。交換事實上將不再需要有任何錢包。這可大大減少盜竊，以及不需要信任的協力廠商管理員。

* 微支付。比特幣區塊鏈手續費太高而不能接受小額支付，尤其是以最小的值。有了這個系統，類似即時的小額支付在沒有一個協力廠商的託管人的情況下使用比特幣將成為可能。這將使支付互聯網服務的每MB或支付報紙的每篇文章成為可能。

* 金融智能合約和託管。金融合約對時間極其敏感並且對區塊鏈計算的要求更高。通過移動絕大多數不可信交易到 off-chain，就可以不通過訪問區塊鏈獲得非常複雜的交易合約條款。


## 12 結論 | Conclusion

Creating a network of micropayment channels enables bitcoin scalability, micropayments down to the satoshi, and near-instant transactions. These channels represent real Bitcoin transactions, using the Bitcoin scripting opcodes to enable the transfer of funds without risk of counterparty theft, especially with long-term miner risk mitigations.

創建小額通道網路使得比特幣具有可擴展性，小額支付，並接近即時交易。這些通道代表真實比特幣交易，使用比特幣腳本操作代碼，使資金不受對方盜取資金的風險而轉移，特別是有長期礦工風險緩解。

---

If all transactions using Bitcoin were on the blockchain, to enable 7 billion people to make two transactions per day, it would require 24GB blocks every ten minutes at best (presuming 250 bytes per transaction and 144 blocks per day). Conducting all global payment transactions on the blockchain today implies miners will need to do an incredible amount of computation, severely limiting bitcoin scalability and full nodes to a few centralized processors.

如果使用比特幣的所有交易都在區塊鏈上，使七十億人每天進行兩筆交易，這最多需要 每十分鐘 24GB 區塊（假設每筆交易 250 Byte，平均每天 144 區塊）。今天開展的所有區塊鏈上的全球支付交易意味著礦工們需要做驚人數量的計算，嚴重限制了比特幣的可 擴展性並且將節點充分的集中到處理器。

---

If all transactions using Bitcoin were conducted inside a network of micropayment channels, to enable 7 billion people to make two channels per year with unlimited transactions inside the channel, it would require 133 MB blocks (presuming 500 bytes per transaction and 52560 blocks per year). Current generation desktop computers will be able to run a full node with old blocks pruned out on 2TB of storage.

如果使用比特幣的所有交易在小額支付的通道網路中進行，以使七十億人創造兩個通道，每年在通道內進行無限的交易，將需要 133 MB 區塊（假設每筆交易 500 Byte，每年 52560 區塊）。目前這一代的臺式電腦將能夠運行一個完整的儲存 2TB 的節點。

---

With a network of instantly confirmed micropayment channels whose payments are encumbered by timelocks and hashlock outputs, Bitcoin can scale to billions of users without custodial risk or blockchain centralization when transactions are conducted securely off-chain using bitcoin scripting, with enforcement of non-cooperation by broadcasting signed multisignature transactions on the blockchain.

隨著即時確認小額支付通道網路的發展，其支付由 timelocks 和 hashlock 輸出控制，比特幣可以在沒有保管風險和區塊鏈集中性的情況下擴展到數十億的使用者，當交易使用比特幣的腳本安全的進行，通過廣播簽署了的區塊鏈上的多重簽名交易來強制實施不合作。

---

## 13 致謝 | Acknowledgements

Micropayment channels have been developed by many parties, and has been discussed on bitcointalk, the bitcoin mailing list, and IRC. The amount of contributors to this idea are immense and much thought have been put into this ability. Effort has been placed into citing and finding similar ideas, however it is absolutely not near complete. In particular, there are many similarities to a proposal by Alex Akselrod by using hashlocking as a method of encumbering a hub-and-spoke payment channel.

小額支付通道已經被多方研究，並在 bitcointalk，Bitcoin 的郵寄清單和 IRC 進行了討論。對這一想法的貢獻者的數量是巨大的，許多想法已經投入到了這一可能性中。人們已經開始致力於尋找類似的想法，雖然這一想法還未被完成。特別是與 Alex Akselrod 的提議，使用哈希碼作為延遲 hub-and-spoke 支付通道的方法有很多相似之處。

---

Thanks to Peter Todd for correcting a significant error in the HTLC script, as well as optimizing the opcode size.

感謝 Peter Todd 在 HTLC 腳本中糾正一個明顯的錯誤，並且優化了操作碼的大小。

---

Thanks to Elizabeth Stark for reviewing and corrections.

感謝 Elizabeth Stark 的審查和修正。

---

Thanks to Rusty Russell for reviewing this document and suggestions for making the concept more digestible, as well as working on a construction which may provide a stop-gap solution before a long-term malleability fix (to be described in a future version).

感謝 Rusty Russell 為使這個概念更易理解，對文檔的審閱和建議，並且致力於創造一個結構，它可以提供在一個長期的可塑性網路出現之前的一個權宜的解決方案（在以後的版本中描述）。

---

## 附錄 A 解決延展性 | Resolving Malleability

In order to create these contracts in Bitcoin without a third party trusted service, Bitcoin must fix the transaction malleability problem. If transactions can be mutated, then signatures can be invalidated, thereby making refund transactions and commitment bonds invalidated. This creates an opportunity for hostile actors to use it as an opportunity for a negotiating tactic to steal coins, in effect, a hostage scenario.

為了在比特幣中創造這些合約，並且不需要協力廠商可信服務，比特幣必須解決交易延展性問題。如果交易可以突變，那麼簽名可以失效，從而使退款交易和承諾債券無效。這將創建一個機會，讓敵對者利用它來談判盜取資金的策略，事實上是 hostage scenario。

---

To mitigate malleability, it is necessary to make a soft-fork change to bitcoin. Older clients would still work, but miners would need to update. Bitcoin has had several soft forks in the past, including pay-to-script-hash (P2SH).

為了減輕延展性，有必要使比特幣的 soft-fork 變化。舊用戶端將仍然有效，但礦工們將需要更新。比特幣在過去有過幾次 soft-fork，包括 pay-to-script-hash（P2SH）。

---

To mitigate malleability, it requires changing which contents are signed by the participants. This is achieved by creating new sighash types. In order to accommodate this new behavior, a new P2SH type or new OP CHECKSIG is necessary to make it a soft-fork rather than a hard-fork.

為了減輕延展性，它需要參與者簽署的這些內容變化。這是通過創建新 sighash 類型來實現的。為了適應這一新的行為，使其成為一個 soft-fork 而不是 hard-fork，一個新 P2SH 型或新的 OP CHECKSIG 是必要的。

---

If a new P2SH was defined, it would use a different output script such as:

```
OP DUP OP HASH160 <20-byte hash> OP EQUALVERIFY
```

如果一個新的 P2SH 被定義，它將使用不同的輸出腳本，例如：

```
OP DUP OP HASH160 <20-byte hash> OP EQUALVERIFY
```

---

Since this will always resolve to true provided a valid redeemScript, all existing clients will return true. This allows the scripting system to construct new rules, including new signature validation rules. At least one new sighash would need to exist.

由於這將在有效地贖回腳本的情況下始終為真，所有現有用戶端將變為真。這使得腳本系統構建新的規則，其中包括新的簽名驗證規則。至少需要一個新的 sighash 存在。

---

SIGHASH NOINPUT would neither sign any input transactions IDs nor sign the index. By using SIGHASH NOINPUT, one can be assured that one’s counterparty cannot invalidate entire trees of chained transactions of potential contract states which were previously agreed upon, using transaction ID mutation. With the new sighash flags, it is possible to spend from a parent transaction even though the transaction ID has changed, so long as the script evaluates as true (i.e. a valid signature).

SIGHASH NOINPUT 既不會簽署任何輸入交易 IDs，也不會簽署其指示。通過使用 SIGHASH NOINPUT，一方可以放心，其交易對方不能使用交易 ID 突變，使之前商定的整個潛在合約中連結交易的目錄樹失效。有了新的 sighash 標誌，即使該交易 ID 發生了改變，也能夠從一個父交易中花費，只要腳本評估為真（有效的簽名）。

---

SIGHASH NOINPUT implies significant risk with address reuse, as it can work with any transaction in which the sigScript returns as valid, so multiple transactions with the same outputs are redeemable (provided the output values are less).

SIGHASH NOINPUT 意味著地址重用的風險，因為只要 sigScript 返回是有效的，他就可以工作，所以有相同輸出的多個交易是可贖回的（如果輸出值變小）。

---

Further, and just as importantly, SIGHASH NOINPUT permits participants to sign spends of transactions without knowing the signatures of the transaction being spent. By solving malleability in the above manner, two parties may build contracts and spend transactions without either party having the ability to broadcast that original transaction on the blockchain until both parties agree. With the new sighash type, participants may build potential contract states and potential payout conditions and agree upon all terms, before the contract may be paid, broadcast, and executed upon without the need for a trusted third party.

另外，同樣重要的是，SIGHASH NOINPUT 允許參與者在不知道該交易的簽署花費的情況下簽署花費交易。通過上述方式解決延展性，雙方可以建立合約和花費交易，而沒有任何一方有在區塊鏈上廣播的原始交易的能力，直到雙方同意。有了新的 sighash 類型，參與者在合約可支付，廣播，並且在無需受信任的協力廠商得條件下執行之前，可以建立一個潛在的合約狀態和潛在的支付條件，並同意所有條款。

---
Without SIGHASH NOINPUT, one cannot build outputs before the transaction can be funded. It is as if one cannot make any agreements without committing funds without knowing what one is committing to. SIGHASH NOINPUT allows one to build redemption for transactions which do not yet exist. In other words, one can form agreements before funding the transaction if the output is a 2-of-2 multisignature transaction.

如果沒有 SIGHASH NOINPUT，不能在為交易集資之前生成輸出交易。這是因為如果一方不知道他將錢投給誰，就不會投資，也就不能達成協議。SIGHASH NOINPUT 允許一方贖回還未生效的交易。換言之，一方能在為交易集資之前生成輸出交易，如果輸出是 2-of-2 的多重簽名交易。

---

To use SIGHASH NOINPUT, one builds a Funding Transaction, and does not yet sign it. This Funding Transaction does not need to use SIGHASH NOINPUT if it is spending from a transaction which has already been entered into the blockchain. To spend from a Funding Transaction with a 2-of-2 multisignature output which has not yet been signed and broadcast, however, requires using SIGHASH NOINPUT.

要使用 SIGHASH NOINPUT，一方建立一個資金交易，並且不簽署。這個資金交易並不需要使用 SIGHASH NOINPUT，如果從已經被廣播到區塊鏈上的交易中花費。要從輸出是 2-of-2 的多重簽名資金交易中花費，此交易還未被簽署和廣播，需要使用 SIGHASH NOINPUT。

---

A further stop-gap solution using OP CHECKSEQUENCEVERIFY or a less-optimal use of OP CHECKLOCKTIMEVERIFY will be described in a future paper by Rusty Russell. An updated version of this paper will also include these constructions.

使用OP CHECKSEQUENCEVERIFY 或另一個不太可取得選擇，即使用 OP CHECKLOCKTIMEVERIFY 的一種權宜的解決方案將通過 Rusty Russell 在以後的論文中描述。本文的更新版本也將包括這些結構。

## 參考文獻 | References
[1] Satoshi Nakamoto. Bitcoin: A Peer-to-peer Electronic Cash System.https://bitcoin.org/bitcoin.pdf, Oct 2008.

[2] Manny Trillo.	Stress Test Prepares VisaNet for the Most Wonderful Time of the Year. http://www.visa.com/blogarchives/us/2013/10/10/stress-test-prepares-visanet-for-the-most-wonderful-time-of-the-year/ index.html, Oct 2013.

[3] Bitcoin Wiki. Contracts: Example 7: Rapidly-adjusted (micro)payments to a pre-determined party. https://en.bitcoin. it/wiki/Contracts#Example_7:Rapidly-adjusted.28micro.29payments_to_a_pre-determined_party.

[4] bitcoinj. Working with micropayment channels. https://bitcoinj. github.io/working-with-micropayments.

[5] Leslie Lamport. The Part-Time Parliament. ACM Transactions on Computer Systems, 21(2):133–169, May 1998.

[6] Leslie Lamport. Time, Clocks, and the Ordering of Events in a Distributed System. Communications of the ACM, 21(7):558–565, Jul 1978.

[7] Alex Akselrod. Draft. https://en.bitcoin.it/wiki/User: Aakselrod/Draft, Mar 2013.

[8] Alex Akselrod. ESCHATON. https://gist.github.com/aakselrod/ 9964667, Apr 2014.

[9] Peter Todd. Near-zero fee transactions with hub-and-spoke micropayments. http://sourceforge.net/p/bitcoin/mailman/message/ 33144746/, Dec 2014.

[10] C.J. Plooy. Combining Bitcoin and the Ripple to create a fast, scalable, decentralized, anonymous, low-trust payment network. http://www.ultimatestunts.nl/bitcoin/ripple_bitcoin_ draft_2.pdf, Jan 2013.

[11] BitPay. Impulse. http://impulse.is/impulse.pdf, Jan 2015.

[12] Mark Friedenbach. BIP 0068: Consensus-enforced transaction replacement signaled via sequence numbers (relative locktime). https://github.com/bitcoin/bips/blob/master/bip-0068. mediawiki, May 2015.

[13] Mark Friedenbach BtcDrak and Eric Lombrozo. BIP 0112: CHECKSEQUENCEVERIFY. https://github.com/bitcoin/bips/blob/ master/bip-0112.mediawiki, Aug 2015.

[14] Jonas Schnelli.	What does OP CHECKSEQUENCEVERIFY do?http://bitcoin.stackexchange.com/a/38846, Jul 2015.

[15] Greg Maxwell (nullc). reddit. https://www.reddit.com/r/Bitcoin/ comments/37fxqd/it_looks_like_blockstream_is_working_on_ the/crmr5p2, May 2015.

[16] Gavin Andresen. BIP 0016: Pay to Script Hash. https://github. com/bitcoin/bips/blob/master/bip-0016.mediawiki, Jan 2012.

[17] Pieter Wuille. BIP 0032: Hierarchical Deterministic Wallets. https:// github.com/bitcoin/bips/blob/master/bip-0032.mediawiki, Feb 2012.

[18] Ilja Gerhardt and Timo Hanke. Homomorphic Payment Addresses and the Pay-to-Contract Protocol. http://arxiv.org/abs/1212.3257, Dec 2012.

[19] Nick Szabo. Formalizing and Securing Relationships on Public Networks. http://szabo.best.vwh.net/formalize.html, Sep 1997.