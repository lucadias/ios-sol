# Braindump Solutions

 <i style=float:right;>Luca Dias</i> <br>

## 1. Kommunikation

Es ist je eine URL fur den Abruf von XML- und JSON-Daten gegeben. Code schreiben, um die Daten abzufragen und zu parsen/deserialisieren.


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
    parser = XMLParser(contentsOf: URL(string:          "http://wherever.ch/hslu/iPhoneAdressData.xml")!)!
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
        let json2 = try! JSONSerialization.jsonObject(with: Data(contentsOf: URL(string: "http://wherever.ch/hslu/iPhoneAdressData.json")!), options: JSONSerialization.ReadingOptions.mutableContainers) as? [Any]
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

## 2. Optionals

Die 2 Arten von Optionals beschreiben.

Datentyp Optional: <DatenTyp>?  
Ein Optional-Typ kann auch keinen Wert, also nil sein.  
Nicht optionale Typen dürfen nicht nil sein und verursachen einen Laufzeitfehler.  
Das heisst Klassen, Structs und Enums müssen immer einen Wert != nil haben.  
Bsp. :
```swift
let convNumber : Int = Int(“1234“) 
//Argument ist ein Stringà kann also nicht zugeordnet werden
let convNumber : Int? = Int(“1234“) 
//Zuweisung an Int-Optional möglich

-> Ausgabe von
    let convNumber : Int? = Int(“1234“)
    print(convNumber)

        -> Optional(1234)
```

Zugriff auf Optional-Werte via __Forced-Unwrapping__ (__!__-Operator):  

```swift
let number : Int = convNumber!
```
Mit dem !-Operator sagt man dem Compiler, dass man sicher ist das der Optionalwert != nil ist, falls er doch nil ist gibt es einen __Laufzeitfehler__.

## 3. Storyboards, ViewControllers

**Das Storyboard beschreiben und den Zusammenhang zwischen Storyboard und Code (UIViewController) erklären. Beschreiben, wie IBOutlets/IBActions erstellt werden können.**

Storyboards sind die visuelle Repräsentation der UserInterfaces einer iOS-Applikation. Stellt verschiedene Szenen (Scene) und die Verbindungen (Seque) dazwischen dar.

Über IBOutlet& IBAction kann man das Layout (Storyboard) mit dem Code (UIViewController) verbinden.

Deklaration über ``` @IBOutlet weak var myLabel: UILabel! ```. Anschliessend mit **ctrl** + Mausklick in Code ziehen. Falls IBOutlet noch nicht vorhanden, wird dieser direkt erstellt.

IBAction verbindet eine Funktion. Achtung keinen Rückgabewert (Rückgabetyp *void*).

## 4. TableView, hierachische Navigation

Das Zusammenspiel von UITableView, UITableViewDelegate und UITableViewDataSource erklären. Den vollständigen Code schreiben um eine View in einem UINavigationViewController zu öffnen und
schliessen. Entsprach mehr oder weniger der Ubung 4.

## 5. Nebenläufigkeit

Grand Central Dispatch und Dispatch Queues beschreiben und deren Unterschied erklären.

## 6 Persistenz

Code schreiben um einen Integer-Wert aus den UserDefaults abzurufen, hochzuz¨ahlen und wieder in den UserDefaults zu speichern.
