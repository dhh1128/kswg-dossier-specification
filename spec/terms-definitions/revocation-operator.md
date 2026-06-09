[[def: revocation-operator, revocation operator]]

~ A weighted operator placed in an edge group's `o` field to define when a revocation is satisfied, configurable independently of the issuance threshold. Like the issuance [[ref: threshold-operator, threshold operators]], its endorsed slots' weights (`w`) must sum to unity (1). The revocation threshold operators are `RMxN` and `RMxQ`, which mirror the mechanics of `MxN` and `MxQ`.
