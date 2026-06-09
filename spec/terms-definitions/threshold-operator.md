[[def: threshold-operator, threshold operator]]

~ A weighted operator placed in an edge group's `o` field to define when a joint issuance is satisfied. Each member slot carries a weight (`w`), and the operator is satisfied when the weights of its endorsed slots sum to at least unity (1) — the same fractionally weighted threshold KERI uses for `kt`. The issuance threshold operators are `MxN` (n named candidate endorsers) and `MxQ` (an open-ended set of qualified endorsers). See also [[ref: revocation-operator]].
