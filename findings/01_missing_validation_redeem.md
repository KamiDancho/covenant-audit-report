[01] Missing Validation for Contract Address in redeem()

Contract Name: Covenant.sol

Function Name: redeem

Description:
The redeem() function allows users to specify any address as the redemption recipient (redeemParams.to).
However, the function does not prevent sending redeemed base tokens to the Covenant contract itself (address(this)).
If a user sets to = address(this), the function transfers tokens to the contract while also decreasing marketState.baseSupply.
This creates an accounting mismatch — the internal supply is reduced, but the actual ERC20 balance inside the contract does not decrease.
Over time, this can desynchronize internal accounting, lead to locked funds, or disrupt later operations that depend on accurate balances.

Mitigation:
Add a validation to prevent self-transfer before the base token transfer occurs:

if (redeemParams.to == address(this)) revert Errors.E_Unauthorized();
IERC20(mp.baseToken).safeTransfer(redeemParams.to, amountOut);
Impact:
Incorrect internal accounting and potential fund locking within the contract.

[02] Unnecessary State Write and Event Emission in setDefaultFee

Contract Name: Covenant.sol

Function Name: setDefaultFee

Description:
The setDefaultFee() function updates _defaultProtocolFee and emits an event even when the new fee value is identical to the current one.

_defaultProtocolFee = newFee;
emit Events.UpdateDefaultProtocolFee(oldFee, newFee);
When newFee == _defaultProtocolFee, this results in:

A redundant storage write (SSTORE) that consumes unnecessary gas.

A redundant event emission, generating noise in off-chain logs.

This has no functional impact but introduces avoidable inefficiency.

Mitigation:
Add a conditional check to skip the write and event emission if the fee remains unchanged:

if (newFee == _defaultProtocolFee) return;

uint32 oldFee = _defaultProtocolFee;
_defaultProtocolFee = newFee;
emit Events.UpdateDefaultProtocolFee(oldFee, newFee);
Impact:
Minor gas overhead and unnecessary log entries without affecting functionality.
