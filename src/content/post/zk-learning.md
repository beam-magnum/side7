---
title: "Notes on Zero Knowledge Proofs"
description: "Things learnt recently learnt about zero knowledge proofs."
author: gingerbeam
publishDate: "17 Jan 2025"
tags: ["cyptography", "zero-knowledge-proofs"]
updatedDate: 6 Mar 2025
---

算术电路: $C: \mathbb{F}^n → \mathbb{F}$.

举例: 验证hash函数的算术电路: $C_{SHA}(h, m)$: 当$SHA(m) = h$输出$0$, 否则输出不为$0$.
可以表示为: $C_{SHA}(h, m) = h - SHA(m)$, 其中$h$和$m$是算术电路的输入. 对于一个SHA256的验证, 算术电路大概需要$20K$个门.

### NARK: Non-interactive ARgument of Knowledge

组成:
- (public) 验证电路: $C(x, w) → \mathbb{F}$;
- (public) 公共声明 (public statement): $x$;
- (private) 证据/见证 (witness): $w$;
- (public) 公共参数 (public parameters): $(pp, vp) \gets S(C)$;
- (public) prover's parameters: $pp$;
- (public) verifier's parameters: $vp$.

<!-- ![alt text](image.png) -->

性质:
- 完备性(Completeness): 对于使得算术电路$C(x, w)$输出$0$的输入, 生成的证明一定可以通过验证.
- 可靠性(Soundness): 如果V接受了证明, 则生成证明的Prover一定有$w$的知识.


SNARK: Succinct NARK, 一个符合完备性, 可靠性的NARK, 同时是简短的.

zk-SNARK: 一个 SNARK, 同时需要满足 zero knowledge.

