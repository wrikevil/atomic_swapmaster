Atomic Swap Module
This module implements an atomic swap mechanism on the Aptos blockchain. The atomic swap allows two users to exchange different types of coins (CoinX and CoinY) in a trustless manner.

Overview
Atomic Swap Mechanics
Initialization: A user initiates the swap by depositing an amount of CoinX and specifying the amount of CoinY they wish to receive in return. This creates an escrow account.
Completion: Another user can then complete the swap by depositing the required amount of CoinY into the escrow account. The system will then transfer CoinX to the completing user and CoinY to the initiating user.
Key Components
1. Escrow Struct
This struct represents the escrow account where the coins are held. It includes:

coin_x: The CoinX instance.
amt_x: The amount of CoinX deposited.
coin_y: The CoinY instance.
req_y: The amount of CoinY requested in exchange for CoinX.
escrow_signer_cap: Capability for the escrow signer.
2. EscrowEvent Struct
Stores the address of the escrow account, allowing users to reference the escrow when performing swaps.

Functions
1. init_escrow<X, Y>(init_user: &signer, seed: vector<u8>)
Initializes the escrow account. This function:

Registers CoinX and CoinY for the init_user.
Creates a resource account for the escrow.
Sets up the Escrow resource with zero balances and an empty request.
2. init_swap<X, Y>(init_user: &signer, amt_x: u64, amt_y: u64)
Initiates a swap by:

Depositing amt_x of CoinX into the escrow.
Recording the requested amt_y of CoinY that the init_user expects in return.
Ensures that CoinY is registered in the init_user's account if not already.
3. complete_swap<X, Y>(comp_user: &signer, init_user_addr: address, amt_y: u64)
Completes the swap by:

Depositing amt_y of CoinY into the escrow.
Transferring the deposited amount of CoinX to the completing user and CoinY to the initiating user if amt_y matches the requested amount.
Error Codes
ECOIN_NOT_INIT: Coin is not initialized in the user's account.
EESCROW_NOT_INIT: Escrow account is not initialized.
EINCORRECT_BALANCE: The coin balance is not as expected.
Tests
The module includes an end-to-end test to ensure the atomic swap process works as expected. It:

Initializes CoinX and CoinY.
Registers and mints the coins for two users.
Tests the initialization of the escrow.
Tests initiating a swap and completing it, verifying balances before and after the swap.
