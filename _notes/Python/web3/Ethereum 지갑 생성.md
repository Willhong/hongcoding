```python
import os
import binascii
from ecdsa import SigningKey, SECP256k1
from web3 import Web3

def generate_private_key():
    return binascii.hexlify(os.urandom(32)).decode('utf-8')

def get_public_key(private_key):
    sk = SigningKey.from_string(binascii.unhexlify(private_key), curve=SECP256k1)
    return binascii.hexlify(sk.verifying_key.to_string()).decode('utf-8')

def keccak256(data):
    return Web3.keccak(data)

def get_eth_address(public_key):
    key_bytes = binascii.unhexlify(public_key)
    keccak_hash = keccak256(key_bytes)
    # Take the last 20 bytes/40 characters
    decapitalized_address ="0x" + binascii.hexlify(keccak_hash[-20:]).decode('utf-8')

    return  Web3.to_checksum_address(decapitalized_address)

def generate_wallet():
    private_key = generate_private_key()
    public_key = get_public_key(private_key)
    address = get_eth_address(public_key)
    return private_key, public_key, address
```