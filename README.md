# EcoChain-Blockchain-Based-Sustainable-Supply-Chain-Management
EcoChain uses Web3 technology to track and verify the sustainability of products throughout their lifecycle, promoting eco-friendly practices and consumer transparency.
import hashlib
import json
from time import time

class Block:
    def __init__(self, index, transactions, timestamp, previous_hash):
        self.index = index
        self.transactions = transactions
        self.timestamp = timestamp
        self.previous_hash = previous_hash
        self.nonce = 0
        self.hash = self.compute_hash()

    def compute_hash(self):
        block_string = json.dumps(self.__dict__, sort_keys=True)
        return hashlib.sha256(block_string.encode()).hexdigest()

class Blockchain:
    def __init__(self):
        self.unconfirmed_transactions = []
        self.chain = []
        self.create_genesis_block()

    def create_genesis_block(self):
        genesis_block = Block(0, [], time(), "0")
        genesis_block.hash = genesis_block.compute_hash()
        self.chain.append(genesis_block)

    def add_new_transaction(self, transaction):
        self.unconfirmed_transactions.append(transaction)

    def add_block(self, block, proof):
        previous_hash = self.last_block.hash

        if previous_hash != block.previous_hash:
            return False

        if not self.is_valid_proof(block, proof):
            return False

        block.hash = proof
        self.chain.append(block)
        return True

    @staticmethod
    def is_valid_proof(block, block_hash):
        return (block_hash.startswith('0'*Blockchain.difficulty) and
                block_hash == block.compute_hash())

    def mine(self):
        if not self.unconfirmed_transactions:
            return False

        last_block = self.last_block

        new_block = Block(index=last_block.index + 1,
                          transactions=self.unconfirmed_transactions,
                          timestamp=time(),
                          previous_hash=last_block.hash)

        proof = new_block.compute_hash()
        self.add_block(new_block, proof)
        self.unconfirmed_transactions = []
        return new_block.index

    @property
    def last_block(self):
        return self.chain[-1]

    def display_chain(self):
        for block in self.chain:
            print(f"Block {block.index}:")
            print(f"Timestamp: {block.timestamp}")
            print(f"Transactions: {block.transactions}")
            print(f"Current Hash: {block.hash}")
            print(f"Previous Hash: {block.previous_hash}\n")

# Example usage
eco_chain = Blockchain()

# Adding sustainability details as transactions
eco_chain.add_new_transaction({"product_id": "1234", "action": "Manufactured", "details": "Used sustainable materials"})
eco_chain.add_new_transaction({"product_id": "1234", "action": "Shipped", "details": "Carbon-neutral shipping method"})

# Mining to confirm transactions
eco_chain.mine()

# Displaying the blockchain
eco_chain.display_chain()
