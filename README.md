# ethereum-custom-interaction   
The ethereum-custom-interaction script facilitates custom interactions with the Ethereum blockchain, providing developers with a versatile tool for engaging with smart contracts and exploring blockchain data.

from web3 import Web3

class EthereumCustomInteractionScript:
    def __init__(self, rpc_url, private_key):
        self.web3 = Web3(Web3.HTTPProvider(rpc_url))
        self.account = self.web3.eth.account.privateKeyToAccount(private_key)

    def deploy_smart_contract(self, contract_abi, contract_bytecode, gas_limit, gas_price):
        # Deploy a smart contract on the Ethereum blockchain
        contract = self.web3.eth.contract(abi=contract_abi, bytecode=contract_bytecode)
        transaction_data = contract.constructor().buildTransaction({
            'from': self.account.address,
            'gas': gas_limit,
            'gasPrice': gas_price,
            'nonce': self.web3.eth.getTransactionCount(self.account.address)
        })

        signed_transaction = self.web3.eth.account.sign_transaction(transaction_data, private_key=self.account.privateKey)
        transaction_hash = self.web3.eth.sendRawTransaction(signed_transaction.rawTransaction)
        print(f"Smart Contract deployed with transaction hash: {transaction_hash.hex()}")

    def call_contract_function(self, contract_address, contract_abi, function_name, function_args):
        # Call a function on an existing smart contract
        contract = self.web3.eth.contract(address=contract_address, abi=contract_abi)
        transaction_data = contract.functions[function_name](*function_args).buildTransaction({
            'from': self.account.address,
            'gas': 100000,
            'gasPrice': self.web3.toWei(10, 'gwei'),
            'nonce': self.web3.eth.getTransactionCount(self.account.address)
        })

        signed_transaction = self.web3.eth.account.sign_transaction(transaction_data, private_key=self.account.privateKey)
        transaction_hash = self.web3.eth.sendRawTransaction(signed_transaction.rawTransaction)
        print(f"Function {function_name} called on contract {contract_address} with transaction hash: {transaction_hash.hex()}")

# Example Usage
ethereum_rpc_url = "https://mainnet.infura.io/v3/your_infura_project_id"
your_private_key = "your_private_key_here"

ethereum_script = EthereumCustomInteractionScript(ethereum_rpc_url, your_private_key)

# Deploy a smart contract (example: ERC20 token)
# contract_abi and contract_bytecode should be replaced with the actual ABI and bytecode of the smart contract
ethereum_script.deploy_smart_contract(contract_abi, contract_bytecode, gas_limit=3000000, gas_price=1000000000)

# Call a function on an existing smart contract
# Replace contract_address, contract_abi, function_name, and function_args with actual values
ethereum_script.call_contract_function(contract_address, contract_abi, function_name, function_args)

This script provides a flexible interface for custom interactions with the Ethereum blockchain. Users can deploy smart contracts and call functions on existing contracts, offering versatility for Ethereum developers exploring the capabilities of the blockchain.
