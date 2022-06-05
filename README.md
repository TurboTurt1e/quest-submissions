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
```
access(all) contract SomeContract {
    pub var testStruct: SomeStruct

    pub struct SomeStruct {

        //
        // 4 Variables
        //

        pub(set) var a: String

        pub var b: String

        access(contract) var c: String

        access(self) var d: String

        //
        // 3 Functions
        //

        pub fun publicFunc() {}

        access(contract) fun contractFunc() {}

        access(self) fun privateFunc() {}


        pub fun structFunc() {
            /**************/
            /*** AREA 1 ***/
            /**************/
            
            // a - can read and write
            // b - can read and write
            // c - can read and write
            // d - can read and write
            // publicFunc   - can be called
            // contractFunc - can be called
            // privateFunc  - can be called
        }

        init() {
            self.a = "a"
            self.b = "b"
            self.c = "c"
            self.d = "d"
        }
    }

    pub resource SomeResource {
        pub var e: Int

        pub fun resourceFunc() {
            /**************/
            /*** AREA 2 ***/
            /**************/
            
            // a - can both read and write
            // b - can read but cannot write
            // c - can read but cannot write
            // d - cannot read and cannot write
            // publicFunc   - can be called
            // contractFunc - can be called
            // privateFunc  - cannot be called

        }

        init() {
            self.e = 17
        }
    }

    pub fun createSomeResource(): @SomeResource {
        return <- create SomeResource()
    }

    pub fun questsAreFun() {
        /**************/
        /*** AREA 3 ****/
        /**************/

        // a - can read and write
        // b - can read and write
        // c - can read but cannot write
        // d - cannot read and cannot write
        // publicFunc   - can be called
        // contractFunc - can be called
        // privateFunc  - cannot be called
    }

    init() {
        self.testStruct = SomeStruct()
    }
}

```
This is a script that imports the contract above:

```
import SomeContract from 0x01

pub fun main() {
  /**************/
  /*** AREA 4 ***/
  /**************/
  // a - can read and write
  // b - can read but cannot write
  // c - cannot read and cannot write
  // d - cannot read and cannot write
  // publicFunc   - can be called
  // contractFunc - cannot be called
  // privateFunc  - cannot be called

}
```

## Chapter 4 Day 1
1) Explain what lives inside of an account. Contract Code and Account Storage and live inside the account. Contract Code is one or more smart contracts containing executable code to be run on the blockchain's virtual machine. Account Storage is where all of your data gets stored.

2) What is the difference between the /storage/, /public/, and /private/ paths?  

The /storage/ path is only accessible by the account owner.
The /public/ path is available to anybody.
The /private/ path is only accessible by the account owner and users that the account owner grants access to.

3) What does .save() do? What does .load() do? What does .borrow() do? 

save stores something to the account storage.
load retrieves something from the account storage.
borrow retrieves a reference to something in the account storage so that it can be inspected.


4) Explain why we couldn't save something to our account storage inside of a script.
Since we are only able to read data from a script, we cannot save something to our account storage inside of a script.

5) Explain why I couldn't save something to your account.
Since you have not been granted permission, you cannot save something to my account.

6) Define a contract that returns a resource that has at least 1 field in it. Then, write 2 transactions:


```
pub contract resourcePractice {

    pub resource MyResource{
        pub let name: String
        pub let description: String
        init(_name:String, _description:String) {
            self.name = _name
            self.description = _description
        }
    }

    pub fun generateMyResource(Name: String, Description: String): @MyResource
    {
        return <- create MyResource(_name: Name, _description: Description)

    }
    init(){
        
    }
}

```

A transaction that first saves the resource to account storage, then loads it out of account storage, logs a field inside the resource, and destroys it.

```
import resourcePractice from 0x01

transaction(name: String, description: String) {

  prepare(signer: AuthAccount) {
    let newResource <- resourcePractice.generateMyResource(Name: name, Description: description)
    signer.save(<- newResource, to: /storage/MyResource)

    let loadedResource <- signer.load<@resourcePractice.MyResource>(from: /storage/MyResource)?? panic("error")
    log(loadedResource.name)
    log(loadedResource.description)
    destroy loadedResource

  }

  execute {
    
  }
}
```

A transaction that first saves the resource to account storage, then borrows a reference to it, and logs a field inside the resource.

```
import resourcePractice from 0x01

transaction(name: String, description: String) {

  prepare(signer: AuthAccount) {
    let newResource <- resourcePractice.generateMyResource(Name: name, Description: description)
    signer.save(<- newResource, to: /storage/MyResource)

    let borrowedResource = signer.borrow<&resourcePractice.MyResource>(from: /storage/MyResource)?? panic("error")
    log(borrowedResource.name)
    log(borrowedResource.description)
    

  }

  execute {
    
  }
}

```
## Chapter 4 Day 2

1) What does .link() do? 

.link() can be used to link resources to the public so that other people can have access it.

2) In your own words (no code), explain how we can use resource interfaces to only expose certain things to the /public/ path.

Resource interfaces can be used to enable controlled access to things in /storage/ that would normally be unavailable to other users by exposing access to certain things via the /public/ path.

3) Deploy a contract that contains a resource that implements a resource interface. 

```
pub contract resourcePractice {

    pub resource interface Named {
        pub var name: String
    }

    pub resource MyResource: Named {
        pub var name: String
        pub let description: String
        init(_name:String, _description:String) {
            self.name = _name
            self.description = _description
        }
    }

    pub fun generateMyResource(Name: String, Description: String): @MyResource
    {
        return <- create MyResource(_name: Name, _description: Description)

    }
    init(){
        
    }
}

```

Then, do the following:

In a transaction, save the resource to storage and link it to the public with the restrictive interface.
```
import resourcePractice from 0x01
transaction () {
  prepare(signer: AuthAccount) {
    signer.save(<- resourcePractice.generateMyResource(Name: "Gobbler", Description: "Deadly Turkey"), to: /storage/MyResource)
    signer.link<&resourcePractice.MyResource{resourcePractice.Named}>(/public/MyResource, target: /storage/MyResource)

  }
}
```
Run a script that tries to access a non-exposed field in the resource interface, and see the error pop up.
```
import resourcePractice from 0x01
transaction (address: Address) {
  prepare(signer: AuthAccount) {

  }
  execute {
    let myCapability: Capability<&resourcePractice.MyResource> = getAccount(address).getCapability<&resourcePractice.MyResource>(/public/MyResource)
    let testAccess: &resourcePractice.MyResource = myCapability.borrow() ?? panic("error")
  }
}
```
Run the script and access something you CAN read from. Return it from the script.
```
import resourcePractice from 0x01
pub fun main(address: Address): String {
  let myCapability: Capability<&resourcePractice.MyResource{resourcePractice.Named}> = getAccount(address).getCapability<&resourcePractice.MyResource{resourcePractice.Named}>(/public/MyResource)
  let testAccess: &resourcePractice.MyResource{resourcePractice.Named} = myCapability.borrow() ?? panic("error")
  return testAccess.name
}

```


## Chapter 4 Day 3

1) Why did we add a Collection to this contract? List the two main reasons.

Using a collection, we can store multiple NFTs in one storage path and we can also allow others to deposit NFTs into this collection.

2) What do you have to do if you have resources "nested" inside of another resource? ("Nested resources")

If we have nested resources, we must declare a destroy function and use the destroy keyword to manually destroy any nested resources.

3) Brainstorm some extra things we may want to add to this contract. Think about what might be problematic with this contract and how we could fix it.

Idea #1: Do we really want everyone to be able to mint an NFT? thinking.

We could restrict minting access by implementing an appropriate resource interface or we could require that a user possesses a specific resource in order to gain access to minting functionality.

Idea #2: If we want to read information about our NFTs inside our Collection, right now we have to take it out of the Collection to do so. Is this good?

No it is not good... If we implement a borrow function, we will no longer need to take the NFT out of the collection to read it.


## Chapter 4 Day 4

```
pub contract CryptoPoops {
  pub var totalSupply: UInt64

  // This is an NFT resource that contains a name,
  // favouriteFood, and luckyNumber
  pub resource NFT {
    pub let id: UInt64

    pub let name: String
    pub let favouriteFood: String
    pub let luckyNumber: Int

    //upon creation, set NFT's id, name, favoriteFood, and luckyNumber
    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid

      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber
    }
  }

  // This resource interface will help us to make deposit, getIDs, and borrowNFT functionality publicly available
  pub resource interface CollectionPublic {
    pub fun deposit(token: @NFT)
    pub fun getIDs(): [UInt64]
    pub fun borrowNFT(id: UInt64): &NFT
  }

  // This resource collection will implement the resource interface
  // This resource Collection will enable us to deposit multiple NFTs to a single storage location. 
  pub resource Collection: CollectionPublic {
    
    // this dictionary maps an id to an NFT
    pub var ownedNFTs: @{UInt64: NFT}

    // implement deposit functionality into the Collection
    // this is part of the resource interface
    pub fun deposit(token: @NFT) {
      self.ownedNFTs[token.id] <-! token
    }

    // implement withdraw functionality
    // this is not part of the resource interface
    pub fun withdraw(withdrawID: UInt64): @NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
              ?? panic("This NFT does not exist in this Collection.")
      return <- nft
    }

    // retrieve a list of NFT IDs from a user
    // this is part of the resource interface
    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    // borrow a reference to an NFT
    // this is part of the resource interface
    pub fun borrowNFT(id: UInt64): &NFT {
      return &self.ownedNFTs[id] as &NFT
    }

    // initialize dictionary ownedNFTs to empty 
    init() {
      self.ownedNFTs <- {}
    }
    
    // if the collection is destroyed, destroy the NFTs that are nested resources
    destroy() {
      destroy self.ownedNFTs
    }
  }

  // used to create an empty collection in a user account before they can accept delivery of our NFTs
  pub fun createEmptyCollection(): @Collection {
    return <- create Collection()
  }

  // anyone with the Minter resource can mint NFTs
  pub resource Minter {
    // function to Mint NFTs
    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }

    // function to mint the Minter resource
    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }

  // initialize totalSupply to begin at 0
  // save Minter resource to the account that deployed the contract
  init() {
    self.totalSupply = 0
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}
```

## Chapter 5 Day 1

1) Describe what an event is, and why it might be useful to a client. 

An event is useful for allowing a smart contract to communicate to the outside when something has happened.

2) Deploy a contract with an event in it, and emit the event somewhere else in the contract indicating that it happened.

'''
pub contract SuperCarFactory {

  // define an event here
  pub event SuperCarMinted(id: UInt64)

  pub resource SuperCar {
    pub let id: UInt64
    pub let make: String
    pub let model: String
    pub let year: Int

    init(Make: String, Model: String, Year: Int) {
      self.id = self.uuid
      self.make = Make
      self.model = Model
      self.year = Year

      // broadcast the event to the outside world
      emit SuperCarMinted(id: self.id)
    }
  }
}
'''

3) Using the contract in step 2), add some pre conditions and post conditions to your contract to get used to writing them out.

```

pub contract SuperCarFactory {

  // define an event here
  pub event SuperCarMinted(id: UInt64)

  pub var totalSupply: UInt64

  pub resource SuperCar {
    pub let id: UInt64
    pub let make: String
    pub let model: String
    pub let year: Int

    init(Make: String, Model: String, Year: Int) {
      pre {
      Make.length > 1: "Error... Make is too short."
      Model.length > 0: "Error... Model is too short."
      Year > 1886: "Error... Year is too early."
      Year <= 2022: "Error... Year is too late."
      }
      post {
        before(SuperCarFactory.totalSupply) == SuperCarFactory.totalSupply - 1
      }

      log(Make)
      log(Model)
      log(Year)

      self.id = self.uuid
      self.make = Make
      self.model = Model
      self.year = Year

      SuperCarFactory.totalSupply = SuperCarFactory.totalSupply + 1
      log(SuperCarFactory.totalSupply)
      // broadcast the event to the outside world
      emit SuperCarMinted(id: self.id)
    }
  }

  init() {
    self.totalSupply = 0
  }
}
        
```
4) For each of the functions below (numberOne, numberTwo, numberThree), follow the instructions.

```
pub contract Test {

  // TODO
  // Tell me whether or not this function will log the name.
  // name: 'Jacob'
  pub fun numberOne(name: String) {
    pre {
      name.length == 5: "This name is not cool enough."
    }
    log(name)
  }

  // TODO
  // Tell me whether or not this function will return a value.
  // name: 'Jacob'
  pub fun numberTwo(name: String): String {
    pre {
      name.length >= 0: "You must input a valid name."
    }
    post {
      result == "Jacob Tucker"
    }
    return name.concat(" Tucker")
  }

  pub resource TestResource {
    pub var number: Int

    // TODO
    // Tell me whether or not this function will log the updated number.
    // Also, tell me the value of `self.number` after it's run.
    pub fun numberThree(): Int {
      post {
        before(self.number) == result + 1
      }
      self.number = self.number + 1
      return self.number
    }

    init() {
      self.number = 0
    }

  }

}
```


## Chapter 5 Day 2

## Chapter 5 Day 3


