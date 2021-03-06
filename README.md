# node-red-contrib-edge-trigger
A set of nodes that trigger on edges:
- `rising edge`: Triggers on rising edge compared to a threshold value
- `falling edge`: Triggers on falling edge compared to a threshold value
- `hysteresis`: Triggers on both edges compared to two threshold values building a hysteresis band

![node-appearance](assets/node-appearance.png "Node appearance")  
**Fig. 1:** Node appearance

<a name="installation"></a>
## Installation

<a name="installation_in_node-red"></a>
### In Node-RED (preferred)
* Via Manage Palette -> Search for "node-red-contrib-edge-trigger"

<a name="installation_in_a_shell"></a>
### In a shell
* go to the Node-RED installation folder, e.g.: `~/.node-red`
* run `npm install node-red-contrib-edge-trigger`

<a name="usage"></a>
## Usage

<a name="node_configuration"></a>
### Node Configuration

![node-settings](assets/node-settings.png "Node properties")  
**Fig. 2:** Node properties (example hysteresis node)

Node configuration is quite simple: In the case of the `rising edge` resp. `falling edge` node, you only have to set one threshold value, in case of the `hysteresis` node, you have to set two threshold values and the initial behaviour (*None*, *Rising*, *Falling*, *Any*).



<a name="input"></a>
### Input
Input data are numerical values which are compared against threshold values.
Any other input data types (e.g. string data types) are ignored (see examples: String "12 Monkeys").

<a name="output"></a>
### Output

The nodes compare the numerical value of the incoming `msg.payload` to the value of the previous message. If that value has crossed a specified threshold (given in the *node configuration*) in a specified direction (*rising*, *falling*), the message is forwarded to the nodes output.


#### Rising edge
Given a threshold value, the node `rising edge` will forward its incoming `msg` if the numerical value of its `msg.payload` has increased above the threshold. An example applications is switching on a cooler when a temperature value rises.  

<img src="assets/rising-trigger.png" title="Rising edge" width="550" />

**Fig. 3:** `rising edge` incoming and outgoing messages


#### Falling edge
Given a threshold value, the node `falling edge` will forward its incoming `msg` if the numerical value of its `msg.payload` has dropped below the threshold. An example applications is switching on a heater when a temperature value drops.  

<img src="assets/falling-trigger.png" title="Falling edge" width="550" />

**Fig. 4:** `falling edge` incoming and outgoing messages


#### Hysteresis
The node `hysteresis`combines the functions of the `falling edge` and `rising edge` nodes. It has both rising and falling thresholds, and forwards the incoming `msg` if its numerical value of its `msg.payload` crosses either threshold in the appropriate direction. The outgoing message has its `edge` property set to `rising` or `falling` (see [example 2](#hysteresis_example2) below for the edge property).  
The difference between the two threshold values is called *hysteresis band*.  

<img src="assets/hysteresis-trigger.png" title="Hysteresis" width="650" />

**Fig. 5:** `hysteresis` incoming and outgoing messages


The `hysteresis` node is useful in situations where it would not be desirable to toggle an output repeatedly on and off if a value hovers around a single threshold (e.g. caused by noise). For example, a light that is activated by a luminosity sensor: It may be appropriate to turn the light on when the sensor indicates less than 30 lux, but not to turn it off again until the sensor indicates more than 40 lux.


<a name="examples"></a>
## Examples
***
**Remark**: Example flows are present in the examples subdirectory. In Node-RED they can be imported via the import function and then selecting *Examples* in the vertical tab menue.
***

<a name="rising_falling_example1"></a>
### Example 1: Rising and falling edge nodes

This example shows the behaviour of the `rising edge`and `falling edge` nodes. You can click on the inject nodes to see which values are forwarded and displayed at the debug nodes crossing the different threshold levels.  

<img src="assets/flow-rising-falling.png" title="Rising edge and falling edge example" width="600" />

**Fig. 6:** `rising edge` and `falling edge` example

<details>
  <summary>Click to expand code snippet for the <em><b>example flow</b></em>.</summary>

```javascript
[{"id":"aaa7c9a9.503a78","type":"inject","z":"38d130a0.78811","name":"","topic":"","payload":"1","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":190,"y":180,"wires":[["f3592261.3ae32","7a546816.767b3"]]},{"id":"f3592261.3ae32","type":"rising-edge","z":"38d130a0.78811","name":"rising, threshold = 42","threshold":"42","x":530,"y":180,"wires":[["cce1f276.6a31e8"]]},{"id":"7a546816.767b3","type":"falling-edge","z":"38d130a0.78811","name":"falling, threshold = 2.7183","threshold":"2.7183","x":550,"y":260,"wires":[["db277fe1.16bd08"]]},{"id":"18ce530e.6cf78d","type":"inject","z":"38d130a0.78811","name":"","topic":"","payload":"6.023e23","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":200,"y":300,"wires":[["f3592261.3ae32","7a546816.767b3"]]},{"id":"dc0d676f.c3f468","type":"inject","z":"38d130a0.78811","name":"","topic":"","payload":"-3.14159","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":200,"y":100,"wires":[["f3592261.3ae32","7a546816.767b3"]]},{"id":"a55450a8.4436d","type":"inject","z":"38d130a0.78811","name":"","topic":"","payload":"9.81","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":190,"y":220,"wires":[["f3592261.3ae32","7a546816.767b3"]]},{"id":"5fca1810.25af88","type":"inject","z":"38d130a0.78811","name":"","topic":"","payload":"360","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":190,"y":260,"wires":[["f3592261.3ae32","7a546816.767b3"]]},{"id":"d43d7bf4.8d68","type":"inject","z":"38d130a0.78811","name":"","topic":"","payload":"1.602e-19","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":210,"y":140,"wires":[["f3592261.3ae32","7a546816.767b3"]]},{"id":"369bc84.86b3ab8","type":"inject","z":"38d130a0.78811","name":"","topic":"","payload":"12 Monkeys","payloadType":"str","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":210,"y":360,"wires":[["f3592261.3ae32","7a546816.767b3"]]},{"id":"cce1f276.6a31e8","type":"debug","z":"38d130a0.78811","name":"","active":true,"tosidebar":false,"console":false,"tostatus":true,"complete":"payload","targetType":"msg","x":840,"y":180,"wires":[]},{"id":"db277fe1.16bd08","type":"debug","z":"38d130a0.78811","name":"","active":true,"tosidebar":false,"console":false,"tostatus":true,"complete":"payload","targetType":"msg","x":840,"y":260,"wires":[]},{"id":"f9f5884.e631978","type":"comment","z":"38d130a0.78811","name":"A string leads to no change","info":"","x":270,"y":400,"wires":[]}]
```
</details>




<a name="hysteresis_example2"></a>
### Example 2: Hysteresis node

This example shows the behaviour of the `hysteresis` node.
The example flow looks like this:  

<img src="assets/flow-hysteresis.png" title="Hysteresis node example" width="600" />

**Fig. 7:** `hysteresis` node example

<details>
  <summary>Click to expand code snippet for the <em><b>example flow</b></em>.</summary>

```javascript
[{"id":"5845ed17.5d48cc","type":"hysteresis","z":"38d130a0.78811","name":"hysteresis, 6371, 40074","rising_threshold":"40074","falling_threshold":"6371","initial_edge":"","x":540,"y":680,"wires":[["8da8954f.ea0048"]]},{"id":"739452a2.d7049c","type":"inject","z":"38d130a0.78811","name":"","topic":"","payload":"384400","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":200,"y":760,"wires":[["5845ed17.5d48cc"]]},{"id":"a9a751c.66373b","type":"inject","z":"38d130a0.78811","name":"","topic":"","payload":"2.54","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":190,"y":640,"wires":[["5845ed17.5d48cc"]]},{"id":"43cf5f4c.7f0b88","type":"inject","z":"38d130a0.78811","name":"","topic":"","payload":"30.48","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":190,"y":680,"wires":[["5845ed17.5d48cc"]]},{"id":"75687242.c02a5c","type":"inject","z":"38d130a0.78811","name":"","topic":"","payload":"-11034","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":190,"y":600,"wires":[["5845ed17.5d48cc"]]},{"id":"7b62e0da.5a45b8","type":"inject","z":"38d130a0.78811","name":"","topic":"","payload":"8848","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":190,"y":720,"wires":[["5845ed17.5d48cc"]]},{"id":"8da8954f.ea0048","type":"debug","z":"38d130a0.78811","name":"","active":true,"tosidebar":true,"console":false,"tostatus":true,"complete":"payload","targetType":"msg","x":840,"y":680,"wires":[]}]
```
</details>


#### Example 2 node configuration
The two hysteresis values are 6371 and 40074. The node configuration is shown in the following figure.  
<img src="assets/hysteresis-example-node-configuration.png" title="Hysteresis example node configuration" width="300" />

**Fig. 8:** `hysteresis` example node configuration


#### Example 2 property `msg.edge` output
An output of the nodes `msg.edge` property is shown in the following figure. When going below the lower hysteresis value (6371 in this example), the output shows:  
<img src="assets/hysteresis-edge-property.png" title="Hysteresis node property msg.edge" width="250" />

**Fig. 9:** `hysteresis`  node `msg.edge` property
