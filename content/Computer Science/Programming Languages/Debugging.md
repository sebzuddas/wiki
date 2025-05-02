When using VScode, using the debugger prevents you from having to use print statements throughout your code. The debugger typically uses a `launch.json` file, which is set up as follows:

```json

{

	"version": "0.2.0",
	
	"configurations": [
	
		{
		
		"name": "Python Debugger: Current File",
		
		"type": "debugpy",
		
		"request": "launch",
		
		"program": "/path/to/main.py", // Use this  like to point to your main.py or the default file you want to debug. Use ${file} if you want it to be whatever file you're 'looking' at
		"console": "integratedTerminal",
		
		"python": "/Users/sebastianozuddas/anaconda3/envs/<specific environment>/bin/python", // Use this line to point to your python environment
		
		}
	
	]

}
```