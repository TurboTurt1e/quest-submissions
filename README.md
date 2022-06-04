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

1) In words, list 3 reasons why structs are different from resources. a) Resources cannot be created whenever you want, but Structs can be easily created anywhere. b) Structs can be easily copied, but Resources must be moved. c) You cannot move a Resource into a variable that currently contains a Resource.   
2) Describe a situation where a resource might be better to use than a struct. When implementing an NFT, a resource is better suited to the task than a struct. 
3) What is the keyword to make a new resource? create is the keyword to make a new resource
4) Can a resource be created in a script or transaction (assuming there isn't a public function to create one)? No... A resource can only be created inside the contract.
5) What is the type of the resource below? Jacob
6) 
```
pub contract Test {
    pub resource Jacob {
        pub let rocks: Bool
        init() {
            self.rocks = true
        }
    }
    pub fun createJacob(): @Jacob { 
        let myJacob <- create Jacob() 
        return <- myJacob 
    }
}
```

## Chapter 3 Day 2
1)
```
pub contract resourcePractice {
    pub var resourceArray: @[MyResource]
    pub var resourceDict: @{String: MyResource}

    pub resource MyResource{
        pub let name: String
        init(_name:String) {
            self.name = _name
        }
    }

    pub fun addResourceToArray (resourceToAdd: @MyResource) {
        self.resourceArray.append(<- resourceToAdd)
    }

    pub fun removeResourceFromArray(index: Int): @MyResource {
       return <- self.resourceArray.remove(at: index)
    }

    pub fun addResourceToDict (resourceToAdd: @MyResource) {
        let index = resourceToAdd.name
        self.resourceDict[index] <-! resourceToAdd
    }

    pub fun removeResourceFromDict (index: String): @MyResource {
        let removedResource <- self.resourceDict.remove(key: index)!
        return <- removedResource
    }

    init(){
        self.resourceArray <- []
        self.resourceDict <- {}
    }
}
```

## Chapter 3 Day 3
1) Define your own contract that stores a dictionary of resources. Add a function to get a reference to one of the resources in the dictionary.
```
pub contract resourcePractice {
    pub var resourceDict: @{String: MyResource}

    pub resource MyResource{
        pub let name: String
        init(_name:String) {
            self.name = _name
        }
    }

    pub fun addResourceToDict (resourceToAdd: @MyResource) {
        let index = resourceToAdd.name
        self.resourceDict[index] <-! resourceToAdd
    }

    pub fun removeResourceFromDict (index: String): @MyResource {
        let removedResource <- self.resourceDict.remove(key: index)!
        return <- removedResource
    }

    pub fun getReferenceToDict (index: String):  &MyResource {
        return &self.resourceDict[index] as &MyResource
    }

    init(){
        self.resourceDict <- {}
        self.addResourceToDict(resourceToAdd: <- create MyResource(_name:"rare"))
        self.addResourceToDict(resourceToAdd: <- create MyResource(_name:"super rare"))
        self.addResourceToDict(resourceToAdd: <- create MyResource(_name:"ultra rare"))
        
    }
}
```

2) Create a script that reads information from that resource using the reference from the function you defined in part 1.
```
import resourcePractice from 0x01

pub fun main():  String {
    let referenceHandle = resourcePractice.getReferenceToDict(index: "ultra rare")
    return referenceHandle.name
}
```

3) Explain, in your own words, why references can be useful in Cadence. References can be useful when handling complex objects that need to be saved to the blockchain and moved around the blockchain. 

## Chapter 3 Day 4

1) Explain, in your own words, the 2 things resource interfaces can be used for (we went over both in today's content)
Interfaces can be used to specify implementation requirements and can be used to control access to specific functionality.

2) Define your own contract. Make your own resource interface and a resource that implements the interface. Create 2 functions. In the 1st function, show an example of not restricting the type of the resource and accessing its content. In the 2nd function, show an example of restricting the type of the resource and NOT being able to access its content.

```
pub contract InterfaceTest {
    pub resource interface IName {
       pub let name: String
    }

    pub resource ExtraSpecialResource: IName {
        pub let name: String
        pub let moarData: Int
        init(_name: String, _number: Int) {
            self.name = _name
            self.moarData = _number
        }
    }

    pub fun doThis() {
        let myResource: @ExtraSpecialResource <- create ExtraSpecialResource(_name: "Juice", _number: 2)
        log(myResource.name)
        log(myResource.moarData)
        destroy myResource
    }

    pub fun notThis() {
        let myResource: @ExtraSpecialResource{IName} <- create ExtraSpecialResource(_name: "Milk", _number: 1)
        log(myResource.name)
        // log(myResource.moarData) //error cannot access this
        destroy myResource
    }

}
```

3) How would we fix this code?
```
pub contract Stuff {

    pub struct interface ITest {
      pub var greeting: String
      pub var favouriteFruit: String
      // add changeGreeting to the ITest interface
      pub fun changeGreeting(newGreeting:String): String
    }

    // ERROR:
    // `structure Stuff.Test does not conform 
    // to structure interface Stuff.ITest`
    pub struct Test: ITest {
      pub var greeting: String
      // implement favoriteFruit to conform with the ITest interface
      pub var favouriteFruit: String

      pub fun changeGreeting(newGreeting: String): String {
        self.greeting = newGreeting
        return self.greeting // returns the new greeting
      }

      init() {
        self.greeting = "Hello!"
        // initialize variable to prevent doomsday scenario
        self.favouriteFruit = "Dragon Hearts"
      }
    }

    pub fun fixThis() {
      let test: Test{ITest} = Test()
      let newGreeting = test.changeGreeting(newGreeting: "Bonjour!") // ERROR HERE: `member of restricted type is not accessible: changeGreeting`
      log(newGreeting)
    }
}
```

## Chapter 3 Day 5


## Chapter 4 Day 1

## Chapter 4 Day 2

## Chapter 4 Day 3

## Chapter 4 Day 4



## Chapter 5 Day 1

## Chapter 5 Day 2

## Chapter 5 Day 3


