# GIF Core Contracts

This repository holds the necessary contracts and tools to interact with an existing GIF instance.

## Setup

Clone this repository

```bash
git clone https://github.com/matthiaszimmermann/gif-contracts.git
cd git-contracts
```

Use [Brownie](https://github.com/matthiaszimmermann/brownie-docker) and start a brownie container

```bash
docker run -it --rm -v $PWD:/projects brownie
```

<!-- docker run -it --rm -v $PWD:/app gif-truffle bash -->

Inside the brownie container compile the contracts/interfaces.

Note: the `--all` switch has the effect that necessary dependencies are loaded and compiled too.
Compile will only succees when using `--all`. 
Alternatively you may manually load these packages using `brownie pm install ...`

```bash
brownie compile --all
```

Run the unit tests
```bash
brownie test
```

Start brownie console
```bash
brownie console
>>> 
```

Example brownie console session
```bash
from scripts.instance import GifInstance
from scripts.product import GifTestOracle
from scripts.product import GifTestProduct

print('accounts setup')
owner = accounts[0]
oracleOwner = accounts[1]
productOwner = accounts[2]
consumer1 = accounts[3]
consumer2 = accounts[4]

print('deploy gif instance')
instance = GifInstance(owner)

print('deploy gif test oracle and product')
oracle = GifTestOracle(instance, oracleOwner)
product = GifTestProduct(instance, oracle, productOwner)
productContract = product.getProductContract()

print('check balances')
Wei(consumer1.balance()).to('ether')
Wei(productContract.balance()).to('ether')

print('create policies')
premium = Wei('0.5 ether')
tx1 = productContract.applyForPolicy({'from': consumer1, 'amount': premium})
tx2 = productContract.applyForPolicy({'from': consumer1, 'amount': premium})
policyId1 = tx1.return_value
policyId2 = tx2.return_value
print('ids of created policies:\n{}\n{}'.format(policyId1, policyId2))

print('balances after policy creation')
Wei(consumer1.balance()).to('ether')
Wei(productContract.balance()).to('ether')

print('show events and subcalls for policy creation')
tx.events
tx.subcalls

print('submit claims')
tx_claim1 = productContract.submitClaim(policyId1, {'from': consumer1})
tx_claim2 = productContract.submitClaim(policyId2, {'from': consumer1})

print('show events for claim submission')
tx_claim1.events
tx_claim2.events

pc = instance.getPolicyController()
pc.getPolicy(policyId1)
pc.getPolicy(policyId2)
pc.getClaim(policyId1, 0)
pc.getClaim(policyId2, 0)

```

## Project Structure

The project structure matches with the [Brownie project structure](https://eth-brownie.readthedocs.io/en/stable/structure.html#structure)