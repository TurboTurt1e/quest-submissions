# quest-submissions
Emerald Academy 


## Chapter 1 Day 1

1) What is a blockchain? 
A blockchain is an open public decentralized database. 

2) What is a Smart Contract? 
A smart contract is a small program that lives on the blockchain and contains code for execution on the blockchain's virtual machine.

3) What are the differences between Transactions and Scripts?
Transactions modify the blockchain and cost money. 
Scripts read the blockchain and do not cost money.

## Chapter 1 Day 2

1) The 5 pillars of Cadence:
Safety and Security
Clarity
Approachability
Developer Experience
Resource Oriented Programming

2) Why are these 5 pillars useful?

Hopefully these pillars encourage more readable code with less bugs and enforce standards with respect to resources.


## Chapter 2 Day 1
1)
```
pub contract JacobTucker {

  pub let is: String

  init() {
      self.is = "the best"
  }

}
```
2)
```
import JacobTucker from 0x03

pub fun main(): String {
  return JacobTucker.is
}
```

![Chapter2-01](https://user-images.githubusercontent.com/105320423/171914010-9e0c1573-cc09-4921-ba69-05148085958c.png)


## Chapter 2 Day 2

1) A script can only read information from the blockchain, it cannot commit changes to the blockchain.

2) What does the AuthAccount mean in the prepare phase of the transaction? AuthAccount is a type used to sign transactions. Having access to the AuthAccount type in the prepare phase of the transaction enables us to access data in the user's account.

3) What is the difference between the prepare phase and the execute phase in the transaction? The execute phase cannot access data in the user's account throught the AuthAccount type, but it can call functions and modify data on the blockchain.

This is the hardest quest so far, so if it takes you some time, do not worry! I can help you in the Discord if you have questions.

Add two new things inside your contract:

A variable named myNumber that has type Int (set it to 0 when the contract is deployed)
A function named updateMyNumber that takes in a new number named newNumber as a parameter that has type Int and updates myNumber to be newNumber

```
pub contract HelloWorld {

    pub var greeting: String
    pub var myNumber: Int

    pub fun changeGreeting(newGreeting: String) {
        self.greeting = newGreeting
    }

    pub fun updateMyNumber(newNumber: Int) {
        self.myNumber = newNumber
    }

    init() {
        self.greeting = "Hello, World!"
        self.myNumber = 0
    }
}
```
Add a script that reads myNumber from the contract
```
import HelloWorld from 0x01

pub fun main(): Int {
  return HelloWorld.myNumber
}
```
Add a transaction that takes in a parameter named myNewNumber and passes it into the updateMyNumber function. Verify that your number changed by running the script again.
```
import HelloWorld from 0x01

transaction(myNewNumber: Int) {

  prepare(signer: AuthAccount) {}

  execute {
    HelloWorld.updateMyNumber(newNumber: myNewNumber)
  }
}
```
![Chapter2-02](https://user-images.githubusercontent.com/105320423/171919742-293cc73c-c0e4-4d08-90ed-84a99bf92324.png)
![Chapter2-02b](https://user-images.githubusercontent.com/105320423/171919771-c5171880-1b99-4c0c-9a06-37e53b854672.png)

## Chapter 2 Day 3

1)
```
pub fun main(){
    var myFavs: [String] = ["Santa","Santana","Satan"]
    log(myFavs)
}
```
2)
```
pub fun main() {
    var s : {String:UInt64} = {"LinkedIn":0, "Facebook":1, "Instagram":2, "YouTube":3, "Reddit":5}
    log(s)
    return
}
```
3)
The force-unwrap operator "!" unwraps an optional type. If the value is nil a panic will be thrown, otherwise we will know we have a value and can continue execution.
```
pub fun main(): Int {
    var int1: Int? = 100
    var unwrappedInt1: Int = int1!
    
    var int2: Int? = nil
    var unwrappedInt2: Int = int2! //error cant continue
    return int2!
}
```
4) We are getting the error because we are returning the wrong type. We must use the "!" to unwrap the optional when returning thing[0x03].

```
return thing[0x03]!
```

## Chapter 2 Day 4

1,2,3)
```
pub contract FilterPresets {

    pub var filterPresets: {Int: FilterPreset}
    pub var numPresets: Int

    pub struct FilterPreset {
        pub let cutoff: Int
        pub let resonance: Int
        pub let filterType: String

        init(Cutoff: Int, Resonance: Int, FilterType: String)
        {
            self.cutoff = Cutoff
            self.resonance = Resonance
            self.filterType = FilterType
        }

    }

    pub fun addNewFilterPreset(cutoff: Int, resonance: Int, filterType: String)
    {
        let newFilterPreset = FilterPreset(Cutoff: cutoff, Resonance: resonance, FilterType: filterType)
        self.filterPresets[self.numPresets] = newFilterPreset
        self.numPresets = self.numPresets + 1
    }

    init() {
        self.filterPresets = {}
        self.numPresets = 0
    }
}
```
4)
```
import FilterPresets from 0x01

transaction(cutoff: Int, resonance: Int, filterType: String) {

  prepare(signer: AuthAccount) {}

  execute {
    FilterPresets.addNewFilterPreset(cutoff: cutoff, resonance: resonance, filterType: filterType)
  }
}
```
5)
```
import FilterPresets from 0x01

pub fun main(id: Int): FilterPresets.FilterPreset {
    return FilterPresets.filterPresets[id]!
}
```
## Chapter 3 Day 1

## Chapter 3 Day 2

## Chapter 3 Day 3

## Chapter 3 Day 4

## Chapter 3 Day 5


## Chapter 4 Day 1

## Chapter 4 Day 2

## Chapter 4 Day 3

## Chapter 4 Day 4



## Chapter 5 Day 1

## Chapter 5 Day 2

## Chapter 5 Day 3


