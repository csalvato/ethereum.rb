# Ruby library for  connecting with Ethereum node

A simple library for Ethereum.

## Highlights

* Pure Ruby implementation
* IPC & HTTP Client with batch calls support
* Compile Solidity contracts with solc compiler
* Deploy and call contracts methods
* Contract events

## Compatibility and requirements

* Tested on parity, might work with geth
* Ruby 2.x
* UNIX/Linux environments 

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'ethereum'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install ethereum

## Basic Usage

### IPC Client Connection

```ruby
client = Ethereum::IpcClient.new
```

### Solidity contract compilation and deployment

```ruby
path = "#{ENV['PWD']}/spec/fixtures/SimpleNameRegistry.sol"
init = Ethereum::Initializer.new(path, client)
init.build_all
simple_name_registry_instance = SimpleNameRegistry.new
simple_name_registry_instance.deploy_and_wait(60)
```

or with simplifed syntax:

```ruby
simple_name_registry_instance = Ethereum::Contract.from_file(path, client)
simple_name_registry_instance.deploy_and_wait(60)
```

### Get contract from blockchain

simple_name_registry_instance = Ethereum::Contract.from_blockchain("SimpleNameRegistry", address, abi, client)

### Transacting and Calling Solidity Functions

Solidity functions are exposed using the following conventions: 

```
transact_[function_name](params) 
transact_and_wait_[function_name](params)  
call_[function_name](params)
```

**Example Contract in Solidity**
```
contract SimpleNameRegistry {

  mapping (address => bool) public myMapping;

  function register(address _a, bytes32 _b) {
  }

  function getSomeValue(address _x) public constant returns(bool b, address b) {
  }

}
```

```ruby
simple_name_registry_instance.transact_and_wait_register("0x5b6cb65d40b0e27fab87a2180abcab22174a2d45", "minter.contract.dgx")
simple_name_registry_instance.transact_register("0x385acafdb80b71ae001f1dbd0d65e62ec2fff055", "anthony@eufemio.dgx")
simple_name_registry_instance.call_get_some_value("0x385acafdb80b71ae001f1dbd0d65e62ec2fff055")
simple_name_registry_instance.call_my_mapping("0x385acafdb80b71ae001f1dbd0d65e62ec2fff055")
```

### Run contracts using a different address

```ruby
simple_name_registry_instance.as("0x0c0d99d3608a2d1d38bb1b28025e970d3910b1e1")
```

### Point contract instance to a previously deployed contract

```ruby
simple_name_registry_instance.at("0x734533083b5fc0cd14b7cb8c8eb6ed0c9bd184d3")
```

## Utils rake tasks

```ruby
rake ethereum:contract:compile[path]  # Compile a contract / Compile and deploy contract
rake ethereum:node:mine               # Mine ethereum testing environment for ethereum node
rake ethereum:node:run                # Run morden (production) node
rake ethereum:node:test               # Run testnet node
rake ethereum:test:setup              # Setup testing environment for ethereum node
rake ethereum:transaction:byhash[id]  # Get info about transaction
```

## Debbuging
Logs from communication with node are available under following path:
```
/tmp/ethereum_ruby_http.log
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. 
Make sure `rake ethereum:test:setup` passes before running tests.
Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Ethereum ruby

This library has been forked from [ethereum-ruby](https://github.com/DigixGlobal/ethereum-ruby).

## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

