# Create Block Class
Create the block class with a file called block.js. Each black has a `hash`, `lastHash`, and `timestamp` attribute.
Create block.js

In block.js:
```
class Block {
constructor(timestamp, lastHash, hash, data) {
	this.timestamp = timestamp;
  this.lastHash = lastHash;
  this.hash = hash;
  this.data = data;
}
toString() {
	return `Block -
          Timestamp : ${this.timestamp}
 		      Last Hash : ${this.lastHash.substring(0, 10)}
          Hash      : ${this.hash.substring(0, 10)
          Data      : ${this.data}`;
  }
}
```
Test the new block class. Create dev-test.js alongside block.js:
```
const Block = require(‘./block’);
const block = new Block();
const block = new Block(‘foo’, ‘bar’, ‘zoo’, ‘baz’);
console.log(block.toString())
```
In `package.json`, find the scripts section and add:

`“dev-test”: “nodemon dev-test”`

Then in the command line, run:
`$ npm run dev-test`

# Genesis Block
Every blockchain starts with the “genesis block” - a default dummy block to originate the chain.

Add a static genesis function to the `Block` class in `block.js`;
```
static genesis() {
	return new this('Genesis time', '-----', 'f1r57-h45h', []);
}
```

Back in dev-test.js, test out the new genesis block:
`console.log(Block.genesis().toString());`

`$ npm run dev-test`

# Mine Blocks
Add a function to add a generate a block based off of some provided `data` to store, and a given `lastBlock.` Call the function `mineBlock`. Generating a block is equated to an act of mining since it takes computational power to “mine” the block. Later on, we’ll make the demand for spending computational power more explicit.

Add the `static mineBlock()` function to the `Block` class:
```
static mineBlock(lastBlock, data) {
	const timestamp = Date.now();
	const lastHash = lastBlock.hash;
  const hash = ‘todo-hash’;
  return new this(timestamp, lastHash, hash, data);
}
```
Now test the new `mineBlock` function. In dev-test.js, delete all of the lines except for the first line that requires the `Block` class.
```
const fooBlock = Block.mineBlock(Block.genesis(), 'foo');
console.log(fooBlock.toString());
```

# Setup Blockchain App
Make a directory for the project called sf-chain. Set up a project in node:
```
$ mkdir sf-chain
$ cd-chain
$ npm init -y
```
Install nodemon as a development dependency. Nodemon is a node engine with a live development server.
`$ npm i nodemon --save-dev`

# SHA256 Hash
A hashing function generates a unique value for the combination of data attributes in the block. The hash for a new block is based on its own timestamp, the data it stores, and the hash of the block that came before it.

Install the `crypto-js` module which has the SHA256 (Secure Hashing Algorithm 256-bit) function:
$ npm i crypto-js --save

In block.js, require the sha256 from the `crypto-js` module at the top:
```
const SHA256 = require('crypto-js/sha256');
```

Then add a new static hash function to the Block class:
```
static hash(timestamp, lastHash, data) {
	return SHA256(`${timestamp}${lastHash}${data}`).toString();
}
```

Now replace the ‘todo-hash’ stub in the `mineBlock` function:
```
const hash = Block.hash(timestamp, lastHash, data);
```

Check the command line - `npm run dev-test` should still be running. Notice the generated hash of the block.

# Test Block
To enforce code consistency as the project develops, add a true testing environment. Install Jest as development dependency, a test runner for JavaScript:
`$ npm i jest --save-dev`

Jest finds files with a `test.js` extension. Create a testing file for the block class:
Create block.test.js

The tests in this file will assert that the block attributes are created properly. In block.test.js:
```
const Block = require('./block');

describe('Block', () => {
  let data, lastBlock, block;

  beforeEach(() => {
    data = 'bar';
    lastBlock = Block.genesis();
    block = Block.mineBlock(lastBlock, data);
  });

  it('sets the `data` to match the input', () => {
    expect(block.data).toEqual(data);
  });

  it('sets the `lastHash` to match the hash of the last block', () => {
    expect(block.lastHash).toEqual(lastBlock.hash);
  });
});
```

Add the `test` script. In the “scripts” section of package.json:
```
"test": "jest --watchAll",
```
`$ npm run test`

If all goes well, you should see two green check marks.




