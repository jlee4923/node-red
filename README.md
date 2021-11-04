# node-red

5-6-12 Node-RED WebSocket 노드레드에서 웹소켙 사용하기
소스프로그램
'''
[{"id":"fd6546df.632a18","type":"inject","z":"4d7f7afa.615144","name":"Tick every 5 secs","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"5","crontab":"","once":false,"onceDelay":"","topic":"test","payload":"","payloadType":"date","x":170,"y":180,"wires":[["6525495f.1eecd8"]]},{"id":"370263af.e0203c","type":"websocket out","z":"4d7f7afa.615144","name":"","server":"985ecbc7.67a138","client":"","x":560,"y":180,"wires":[]},{"id":"6525495f.1eecd8","type":"function","z":"4d7f7afa.615144","name":"format time nicely","func":"msg.payload = Date(msg.payload).toString();\nreturn msg;","outputs":1,"noerr":0,"initialize":"","finalize":"","x":370,"y":180,"wires":[["370263af.e0203c"]]},{"id":"734ecc43.6050a4","type":"websocket in","z":"4d7f7afa.615144","name":"","server":"985ecbc7.67a138","client":"","x":360,"y":280,"wires":[["340e1c45.9716c4"]]},{"id":"340e1c45.9716c4","type":"debug","z":"4d7f7afa.615144","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","statusVal":"","statusType":"auto","x":570,"y":280,"wires":[]},{"id":"286f978b.ef2a18","type":"comment","z":"4d7f7afa.615144","name":"https://flows.nodered.org/flow/8666510f94ad422e4765","info":"","x":420,"y":60,"wires":[]},{"id":"9b1de891.a8df68","type":"comment","z":"4d7f7afa.615144","name":"웹소켙","info":"","x":150,"y":60,"wires":[]},{"id":"80d75e74.6d99a","type":"inject","z":"4d7f7afa.615144","name":"","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"테스트","payloadType":"str","x":370,"y":220,"wires":[["370263af.e0203c"]]},{"id":"68eef96b.216138","type":"template","z":"4d7f7afa.615144","name":"Simple Web Page","field":"payload.scriptSocket","fieldType":"msg","format":"html","syntax":"mustache","template":"        var ws;\n        var wsUri = \"ws:\";\n        var loc = window.location;\n        console.log(loc);\n        if (loc.protocol === \"https:\") { wsUri = \"wss:\"; }\n        // This needs to point to the web socket in the Node-RED flow\n        // ... in this case it's ws/simple\n        wsUri += \"//\" + loc.host + loc.pathname.replace(\"simple\",\"ws/simple\");\n\n        function wsConnect() {\n            console.log(\"connect\",wsUri);\n            ws = new WebSocket(wsUri);\n            //var line = \"\";    // either uncomment this for a building list of messages\n            ws.onmessage = function(msg) {\n                var line = \"\";  // or uncomment this to overwrite the existing message\n                // parse the incoming message as a JSON object\n                var data = msg.data;\n                console.log(data);\n                // build the output from the topic and payload parts of the object\n                line += \"<p>\"+data+\"</p>\";\n                // replace the messages div with the new \"line\"\n                document.getElementById('messages').innerHTML = line;\n                //ws.send(JSON.stringify({data:data}));\n            }\n            ws.onopen = function() {\n                // update the status div with the connection status\n                document.getElementById('status').innerHTML = \"connected\";\n                //ws.send(\"Open for data\");\n                console.log(\"connected\");\n            }\n            ws.onclose = function() {\n                // update the status div with the connection status\n                document.getElementById('status').innerHTML = \"not connected\";\n                // in case of lost connection tries to reconnect every 3 secs\n                setTimeout(wsConnect,3000);\n            }\n        }\n        \n        function doit(m) {\n            if (ws) { ws.send(m); }\n        }","x":350,"y":120,"wires":[["2ee6d3d5.235dec"]]},{"id":"2ee6d3d5.235dec","type":"template","z":"4d7f7afa.615144","name":"Main html","field":"payload","fieldType":"msg","format":"html","syntax":"mustache","template":"<!DOCTYPE HTML>\n<html>\n<head>\n    <meta name=\"viewport\" content=\"width=device-width, initial-scale=1\">\n    <script type=\"text/javascript\"> {{{payload.scriptSocket}}} </script>\n</head>\n    <body onload=\"wsConnect();\" onunload=\"ws.disconnect();\">\n        <h1>웹소켙</h1>\n        <div id=\"messages\"></div>\n        <div id=\"status\">unknown</div>\n        <hr/>\n        <button type=\"button\" onclick='doit(\"누름\");'>Click to send message</button>\n    </body>\n</html>\n\n","x":520,"y":120,"wires":[["42a28745.bd5d78"]]},{"id":"42a28745.bd5d78","type":"http response","z":"4d7f7afa.615144","name":"","x":650,"y":120,"wires":[]},{"id":"1787be40.e87842","type":"http in","z":"4d7f7afa.615144","name":"","url":"/simple","method":"get","swaggerDoc":"","x":161,"y":120,"wires":[["68eef96b.216138"]]},{"id":"985ecbc7.67a138","type":"websocket-listener","z":"4d7f7afa.615144","path":"/ws/simple","wholemsg":"false"}]
'''
