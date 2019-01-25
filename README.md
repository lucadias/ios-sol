# Braindump Solutions

 <i style=float:right;>dias</i> <br>

## 1. Kommunikation

_Es ist je eine URL fur den Abruf von XML- und JSON-Daten gegeben. Code schreiben, um die Daten abzufragen und zu parsen/deserialisieren._


### XML Parsen

1. Parser mit gewünschter URL initialieren
2. Delegate von XMLParser setzen
3. Delegate implementieren
4. Parser starten (Methode „parse“)
5. In der Methode „parser(didStartElement:...)“ können nach und nach die Element ausgelesen werden

Code Beispiel:
```swift
func getXMLData() -> [String] {
    tmpXmlStrings = []
    parser = XMLParser(contentsOf: URL(string:                 
        "http://wherever.ch/hslu/iPhoneAdressData.xml")!)!
    parser.delegate = self
    parser.parse()
    return tmpXmlStrings
}
```

### JSON Parsen

Code Beispiel:

```swift
func getJSONData() -> [String] {
        tmpJSONStrings = []
        var tempString = ""
        let json2 = try! JSONSerialization.jsonObject(with: Data(
                contentsOf: URL(
                string: "http://wherever.ch/hslu/iPhoneAdressData.json")!),
                options: JSONSerialization.ReadingOptions.mutableContainers) 
                as? [Any]
        if let array = json2 {
                for object in array {
                    if let dictionary = object as? [String: Any] {
                        if let firstName = dictionary["firstName"] as? String {
                            tempString = firstName
                        }
                        if let lastName = dictionary["lastName"] as? String {
                             tempString +=  " " + lastName
                        }
                    self.tmpJSONStrings.append(tempString)
                    }
                }
            }
            print(self.tmpJSONStrings)
        return self.tmpJSONStrings
    }
```

Doku Auszug aus der Klasse JSONSerialization:

```code
An object that may be converted to JSON must have the following properties:

* The top level object is an Array or Dictionary.
* All objects are instances of String, Number, Array, Dictionary, or NSNull.  
* All dictionary keys are instances of String.  
* Numbers are not NaN or infinity.
```

## 2. Optionals

_Die 2 Arten von Optionals beschreiben._

Optional:
es kann sein, dass kein Wert vorhanden ist (__?__-Operator)

```swift
let convNumb : Int? = Int("1234")
```

__Forced-Unwrapping:__  
Zugriff auf Optional-Werte

```swift
let number : Int = convNumb!
```

Mit dem !-Operator sagt man dem Compiler, dass man sicher ist das der Optionalwert != nil ist, falls er doch nil ist gibt es einen __Laufzeitfehler__.

__Implicity Unwrapped Optionals:__  
Optionals, die immer einen Wert haben (sollten)

```swift
let assumedString: String! = "An implicity unwrapped optional string."
let implicitString: String = assumedString // no need for an exclamation mark
```

*Because the value of an implicitly unwrapped optional is automatically unwrapped when you use it, there’s no need to use the ! operator to unwrap it. That said, if you try to use an implicitly unwrapped optional that has a value of nil, you’ll get a runtime error.*


## 3. Storyboards, ViewControllers

_Das Storyboard beschreiben und den Zusammenhang zwischen Storyboard und Code (UIViewController) erklären. Beschreiben, wie IBOutlets/IBActions erstellt werden können._

## 4. TableView, hierachische Navigation

_Das Zusammenspiel von UITableView, UITableViewDelegate und UITableViewDataSource erklären. Den vollständigen Code schreiben um eine View in einem UINavigationViewController zu öffnen und
schliessen. Entsprach mehr oder weniger der Ubung 4._

## 5. Nebenläufigkeit

_Grand Central Dispatch und Dispatch Queues beschreiben und deren Unterschied erklären._

2 mächtige Mechanismen für asynchrone Tasks

* __GCD: Grand Central Dispatch__
* __Operation Queues__

### Grand Central Dispatch

Grand Central Dispatch (GCD) comprises language features, runtime libraries, and system enhancements that provide systemic, comprehensive improvements to the support for concurrent code execution on multicore hardware in iOS and Mac OS X.

GCD: Beispiel-Code:

```swift
let myQueue = DispatchQueue(label: "ch.hslu.ios.MyQueue")
myQueue.async { print("Do some work here.") }
print("The first block may or may not have run.")
myQueue.sync { print("Do some more work here.") }
print("Both blocks have completed.")
```

Ausgabe:

```code
Do some work here.
The first block may or may not have run.
Do some more work here.
Both blocks have completed.
```

GCD provides and manages FIFO queues to 
which your applicaFon can submit tasks in the 
form of block objects. Blocks submiLed to 
dispatch queues are executed __on a pool of 
threads fully managed by the system.__ No 
guarantee is made as to the thread on which a 
task executes.

### Operation Queues

__Grundidee__

1. Nebenläufige Aufgaben in Operations definieren
2. Operations einer Operation Queue hinzufügen
3. Operation Queue kümmert sich um Operations und führt diese aus

Beispiel Code:

```swift
@IBAction func testOperationQueueButtonPressed(_ sender: UIButton) {
    var orderArray : [String] = []
    //Erstellt 3 Block Operations
    let blockOp1 = BlockOperation{
        orderArray.append("1")
    }
    let blockOp2 = BlockOperation{
        orderArray.append("2")
    }
    let blockOp3 = BlockOperation{
        orderArray.append("3")
    }
    //Definieren der Abhängigkeiten zwischen den Operations
    blockOp1.addDependency(blockOp2)
    blockOp1.addDependency(blockOp3)
    blockOp2.addDependency(blockOp3)
    //Erstellt Operation Queue und fügt Operations hinzu
    let lucasqueueue = OperationQueue() 
    let blockarray : [BlockOperation] = [blockOp1,blockOp2,blockOp3]
    lucasqueueue.addOperations(blockarray, waitUntilFinished: true)
    //Alermessage mit der Auswertung der BlockOperations
    var alertmessage = "The three Block operations were executed in the following order:" + orderArray.description
    let alert = UIAlertController(title: "Block Operation Orderung", message: alertmessage, preferredStyle: .alert)
    alert.addAction(UIAlertAction(title: "Ok, thanks", style: .default, handler: nil))
    self.present(alert, animated: true)
}
```

Ausgabe:  
__Block Operation Ordering__  
The three block operations were executed in the following order: 3, 2, 1.

### GCD vs. Operation Queues

Gründe für __Op-Queues__ könnten sein:

* High(er)-level Abstraktion
* OO: Arbeiten mit Objekten..

Gründe für __GCD__ könnten sein:

* Funktionalität (GCD bietet mehr Möglichkeiten als OperationQueues)
* Performance (GCD weniger Overhead, etwas performanter als OperationQueues)


## 6 Persistenz

_Code schreiben um einen Integer-Wert aus den UserDefaults abzurufen, hochzuzählen und wieder in den UserDefaults zu speichern._

All iOS apps have a built in data dictionary that stores small amounts of user settings for as long as the app is installed. This system, called UserDefaults can save integers, booleans, strings, arrays, dictionaries, dates and more, but you should be careful not to save too much data because it will slow the launch of your app.

Beispiel Code:

```swift
//Get UserDefaults Singleton Object
let defaults = UserDefaults.standard
//Set UserDefault Variables
defaults.set(25, forKey: "Age")
defaults.set(true, forKey: "UseTouchID")
defaults.set(CGFloat.pi, forKey: "Pi")
//Get UserDefaultVaraibles
let age = defaults.integer(forKey: "Age")
let useTouchID = defaults.bool(forKey: "UseTouchID")
let pi = defaults.double(forKey: "Pi")
```

__Achtung:__ Accessor-Methoden nach Datentypen

```swift
.object(forKey: "key")
.integer(forKey: "key")
.string(forKey: "key")
.array(forKey: "key")
.url(forKey: "key")
```
