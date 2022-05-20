# How to build and test this contract

1. Deploy vbi-ft.wasm to an ft_contract_id (`ft.duongnh.testnet`)

   ```
   - near deploy --wasmFile token-test/vbi-ft.wasm ft.duongnh.testnet
   ```

2. Deploy out/staking-contract.wasm to staking_contract_id (`staking.duongnh.testnet`)

   ```
   - ./build.sh
   - near deploy --wasmFile out/staking-contract.wasm staking.duongnh.testnet
   ```

3. Init contract in `ft.duongnh.testnet`

   ```
   near call ft.duongnh.testnet new_default_meta '{"owner_id": "duongnh.testnet", "total_supply": "1000000000000000000000000000000000"}' --accountId duongnh.testnet
   ```

4. Init contract in `staking.duongnh.testnet`

   ```
   near call staking.duongnh.testnet new_default_config '{"owner_id": "duongnh.testnet", "ft_contract_id": "ft.duongnh.testnet"}' --accountId duongnh.testnet
   ```

5. Check pool info:

   ```
   near view staking.duongnh.testnet get_pool_info
   ```

6. Create an account in `staking.duongnh.testnet` (by call contract `storage_deposit`)

   ```
   near call staking.duongnh.testnet storage_deposit --accountId duongnh.testnet --deposit 0.01
   ```

7. Check account info: `duongnh.testnet`

   ```
   near view staking.duongnh.testnet get_account_info '{"account_id": "duongnh.testnet"}'
   ```

8. Create an account in `ft.duongnh.testnet` (by call contract `storage_deposit`)

   ```
   near call ft.duongnh.testnet storage_deposit '{"account_id": "staking.duongnh.testnet"}' --accountId ft.duongnh.t
   estnet --deposit 0.01
   ```

9. Transfer: (call contract `ft_transfer_call` in `ft.duongnh.testnet`)

   ```
   near call ft.duongnh.testnet ft_transfer_call '{"receiver_id": "staking.duongnh.testnet", "amount": "1000000000000000000000000", "msg": ""}' --accountId duongnh.testnet --depositYocto 1 --gas 60000000000000
   ```

10. Check account info again: `duongnh.testnet`

    ```
    near view staking.duongnh.testnet get_account_info '{"account_id": "duongnh.testnet"}'
    ```

11. Harvest reward:

    ```
    near call staking.duongnh.testnet harvest --accountId duongnh.testnet --depositYocto 1 --gas 60000000000000
    ```

12. Unstake:

    ```
    near call staking.duongnh.testnet unstake '{"amount": "1000000000000000000"}' --accountId duongnh.testnet --depositYocto 1
    ```

13. Withdraw (User can only withdraw after 1 epoch since unstake (~12 hours))

    ```
    near call staking.duongnh.testnet withdraw --accountId duongnh.testnet --depositYocto 1 --gas 300000000000000
    ```

14. Run simulation tests:

    ```

    ```
