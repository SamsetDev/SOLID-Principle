# SOLID-Principle

After lots of exploring i found best solution and understanding about SOLID principle. So in this repo we are understand what is SOLID principle and why we are use and what is right use of priciple.

So lets start 
Basically SOLID principle is 5 type --

•	Single Responsibility Principle
•	Open-Close Principle 
•	Liskov Substitution Principle 
•	Interface Segregation Principle 
•	Dependency Inversion Principle 

1. Single Responsbility Principle 

      >  A class has to have only one responsibility.


If our class is doing more than one responsibility, Then we should split our class in such a manner, That one class has only one responsibility and        others have other responsibilities, Else we are violating SRP


```
   data class Employee(
    var empId: String,
    var empName: String,
    var empPhoneNumber: String,

    var empCtc: String,
    var empTakeHome: String,
    var empTax: String,

    var addressLine: String,
    var landmark: String,
    var pinCode: String,
    var city: String,
    var state: String) 
    {
    fun getEmpBasicInformation() {
        println("Emp Id: $empId")
        println("Emp name: $empName")
        println("Emp name: $empName")
        println("Emp PhoneNumber: $empPhoneNumber")
    }
    fun getEmpSalaryInformation() {
        println("Emp ctc: $empCtc")
        println("Emp take home: $empTakeHome")
        println("Emp tax: $empTax")
    }
    fun getEmpAddressInformation() {
        println("Emp address: $addressLine,$landmark,$city,$pinCode,$state")
    }

}
```

Here Employee class is managing more than one responsibility.
<ul>
<li> Employee info </li>
<li> Ctc Info </li>
<li> Address info </li>
</ul>
So updating any of this will take more time as well as in more places, We need to maintain the code, and if we want to reuse one of the subparts so it is not reusable.

See the SRP version of the above code snippet:


```
 data class EmployeeDetail(
    val employee: Employee,
    val employeeCTC: EmployeeCTC,
    val employeeAddress: EmployeeAddress
) {
    fun getEmployee() {
        employee.getEmpBasicInformation()
    }

    fun getEmployeeCTC() {
        employeeCTC.getEmpCTCInformation()
    }

    fun getEmployeeAddress() {
        employeeAddress.getEmpAddressInformation()
    }
}

data class Employee(
    var empId: String,
    var empName: String,
    var empPhoneNumber: String
) {
    fun getEmpBasicInformation(): String {
        return "Employee(empId='$empId'," +
                " empName='$empName', " +
                "empPhoneNumber='$empPhoneNumber')"
    }
}

data class EmployeeCTC(
    var empCtc: String,
    var empTakeHome: String,
    var empTax: String
) {
    fun getEmpCTCInformation(): String {
        return "EmployeeCTC(empCtc='$empCtc'," +
                " empTakeHome='$empTakeHome', " +
                "empTax='$empTax')"
    }
}

data class EmployeeAddress(
    var addressLine: String,
    var landmark: String,
    var pinCode: String,
    var city: String,
    var state: String
) {
    fun getEmpAddressInformation(): String {
        return "EmployeeAddress(addressLine='$addressLine'," +
                " landmark='$landmark'," +
                " pinCode='$pinCode', " +
                "city='$city', state='$state')"
    }
}
```

Above we have separated each class as a responsibility, So the code is cleaner and readable, and maintainable, If we want to use any of the classes independently in many places we can do it easily, and If we want to update any class so it is much easier now.

**2. Open-Closed Principle:**

             >Software entities such as classes, functions, and modules should be open for extension but closed for modification.

If we have already written the class, So it should be open for extension but closed for modification, Hence we should design the class in such a way that follows this principle(for a new feature we should not change the current class rather we should create a new class by extending the current class)

**Let's see the bad example:**

```
class MathematicalOperation {

    fun doCalculation(operationName: String, firstNumber: Int, secondNumber: Int): Int {
        return when (operationName) {
            "Addition" -> {
                //do addition operation here
                firstNumber + secondNumber
            }
            "Subtraction" -> {
                // do subtraction operation
                firstNumber - secondNumber
            }
            else -> {
                throw OperationNotSupportedException()
            }
        }
    }

}
```

In the above example, We have a method that takes operation names and does the operation if we want to increase the operations so in that case, We need to touch the code of the above class and need to modify the doCalculation() method. Hence we are violating the Open close Principle.

**Let's fix the above issue:**

```
abstract class MathematicalOperation {
    abstract fun doCalculation(firstNumber: Int, secondNumber: Int): Int
}

class Addition : MathematicalOperation() {
    
    override fun doCalculation(firstNumber: Int, secondNumber: Int): Int {
        return firstNumber + secondNumber
    }

}

class Subtraction : MathematicalOperation() {

    override fun doCalculation(firstNumber: Int, secondNumber: Int): Int {
        return firstNumber - secondNumber
    }

}

class Multiplication : MathematicalOperation() {

    override fun doCalculation(firstNumber: Int, secondNumber: Int): Int {
        return firstNumber * secondNumber
    }

}
```

Here in the above example, we are having an abstract class which is having the abstract method and all the subclasses are implementing its functionality according to the type of classes, So in the future, any new class will introduce so we need not modify existing code rather we can just define 1 class and its functionality and we are done.

**3. Liskov substitution Principle:**

           Child classes should never break the parent class’ type definitions.

Parent classes should be easily substituted with their child classes without changing the behaviour of the parent classes. It means that a subclass should override the methods from a parent class, which does not break the functionality of the parent class.

This becomes important when we use inheritance, We always need to check if the child class is a proper subset of a parent class then only we should use inheritance otherwise we are breaking LSP.

**Let's see the bad example:**

```
open class Bird {
    fun fly() {}
}
class Parrot : Bird()
class Hen : Bird()
class Hen : Bird()
```

Here in the above example, we have bird class and we have Parrot and Hen subclasses

Bird class has the fly method but the Hen can not fly so it violates the principle

**Let's fix the issue by LSP:**

```
open class Bird {
}

open class FlyingBirds : Bird() {
    fun fly() {}
}

class Parrot : FlyingBirds()
class Hen : Bird()
```

Here we have introduced one more class which has a fly method that is applicable only to the type of bird that can fly.

**3. Interface Segregation Principle:**

The Interface Segregation Principle (ISP) states that a client should not be exposed to methods it doesn’t need. Declaring methods in an interface that the client doesn’t need pollutes the interface and leads to a “bulky” or “fat” interface.
other words >>>>>>>
Interface segregation means if you have one interface and containing multiple methods and after some time you need to add one more method in same interface so you need to implement all over project so you can create separate interface and implement separately

**Let’s see the bad example:**

```
interface Listener{
    fun longClickListener()
    fun onClickListener()
    fun sendDataToOtherPage()
    fun showToast()
    fun clearInput()
}
}
```

Here in the above example, we have only 1 interface and many methods which some classes will not need so we are making a bulky interface that is of not much use for many classes

**Let's fix this:**

```
interface ClickListener{
    fun longClickListener()
    fun onClickListener()
}

interface DataDownloaded{
    fun showResult()
    fun showToast()
}
```

Here we have 2 interfaces at the time of listeners we can use the click listener interface and on data loaded we can use another interface which are having fewer and more relevant methods

**3. Dependency Inversion Principle:**

                Any higher classes should always depend upon the abstraction of the class rather than the detail.
                
Higher classes are not dependent on the lower classes but instead depend upon the abstraction of the lower classes.

**Let's see the bad example:**

```
class Computer(keyboard:DellKeyboard){
    
    fun assembleComputer(){
        //use keyboard and other components
    }
}

class DellKeyboard(){
    
}
}
```

Here In the above example, I am dependent on Dell Keyboard which violates the DI principle

**Let’s Fix the example:**

```
class Computer(keyboard: Keyboard) {

    fun assembleComputer() {
        //use keyboard and other components
    }
}

open class Keyboard {
}

class DellKeyboard : Keyboard() {
}
class HPKeyboard : Keyboard() {
}
```

Here, we are expecting a keyboard but the type is not defined so we can pass any type of keyboard depending on the requirements.
















       
       
       
