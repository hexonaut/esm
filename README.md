# ESM

Emergency Shutdown Module

## Description

The ESM is a contract with the ability to call `end.cage()`, i.e. trigger an
Emergency Shutdown (aka Global Settlement).

MKR holders `join` their funds, which are then immediately burnt. When the ESM's
internal `sum` balance is equal to or greater than the `min` threshold, the ESM
can be `fire`d.

It is meant to be used by an MKR minority to thwart two types of attack:

* malicious governance (using an ESM deployed version which denies the pause proxy on execution).  This contract is named `MCD_ESM_ATTACK` in the changelog.
* critical bug (using an ESM deployed version which doesn't deny the pause proxy on execution).  This contract is named `MCD_ESM_BUG` in the changelog.

In the former case, the pledgers will have no expectation of recovering the
funds (as that would require a malicious majority to pass the required vote),
and their only option is to set up an alternative fork in which the majority's
funds are slashed.

In the latter case, governance can choose to refund the ESM pledgers by minting new
tokens.

If governance wants to disarm the ESM, it can only do so by removing its
authorization to call `end.cage()`.

## Invariants

* `fire` can be called by anyone
* `fire` can be called only once (enforced by `end` module)
* `fire` requires `sum` to be >= `min`
* `join` can only be called before `fire` (enforced by `end` module)
* gems are burnt using the extra function `burn` after `join`ing
