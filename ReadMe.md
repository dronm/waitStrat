**WaitStrategy** manages reconnection strategies. For example data base reconnections, network protocol reconnections etc.<br/>
Several strategies should be defined on construction. Every strategy includes maximum try count and wait interval.<br/>
On failur wait interval can be fetched with *NextWait()* function. For example *time.Sleep(time.Duration(WaitStrategys.NextWait()) * time.Millisecond)*<br/>
Zero try count means endlees try.<br/>
```golang
import "github.com/dronm/waitStrat"

reconParams =: waitStrat.WaitStrategy{
	Strategies: []waitStrat.WaitStrategyValues{
		waitStrat.WaitStrategyValues{10,1000},  // first attempt, try 10 times with 1000 wait value
		waitStrat.WaitStrategyValues{12,10000}, // second 12 times with 10000 wait value  
		waitStrat.WaitStrategyValues{0,30000},  // last endless try with 30000 wait 
	}},
reconnect:
	for !Connected() {
		time.Sleep(time.Duration(reconParams.NextWait()) * time.Millisecond) //get next wait interval
		not_connexted := !Connected()
	}
	reconParams.Init() // on success reset internal counter
```
