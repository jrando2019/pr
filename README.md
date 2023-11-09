## README

### MQTT Graph Data Processor

This Node.js script acts as an MQTT client, handling and processing incoming graph data messages. The processed data is then published to different MQTT topics. The main goal is to manage BTC (Bitcoin) graph data, specifically torque, angle, and current data.

#### Requirements

- **Node.js**: Make sure you have Node.js installed.
- **MQTT Broker**: Ensure that an MQTT broker is running at `mqtt://mqtt-broker:1883`. You can customize the broker URL and configuration in the script.
- 
#### Code Walkthrough

1. **MQTT Connection Setup:**

   ```javascript
   const client = mqtt.connect('mqtt://mqtt-broker:1883', {
     reconnectPeriod: 5000, // Reconnect every 5 seconds
   });
   ```

   Set up the MQTT connection with the specified broker and configuration.

2. **Message Subscription:**

   ```javascript
   client.subscribe('BTC/notifications/BTC.graphdata', (err) => {
     if (!err) {
       console.log('Subscribed to BTC/notifications/BTC.graphdata');
     }
   });
   ```

   Subscribe to the 'BTC/notifications/BTC.graphdata' topic to receive incoming graph data messages.

3. **Data Parsing:**
   
    The script begins by attempting to parse the incoming JSON message:
   
    ```javascript
    try {
    if (topic === 'BTC/notifications/BTC.graphdata') {
        // Data parsing and transformation logic goes here
        // ...
     }
    } catch (error) {
    console.error('An error occurred:', error);
    // Handle the error as needed, and optionally continue processing
    }
    ```
    
    If the topic matches 'BTC/notifications/BTC.graphdata', it proceeds with parsing the JSON message. Any errors during this process are caught and logged.

5. **Graph Data Processing:**
   
    The core of the data processing logic involves extracting parameters and iterating through the graph data to convert raw values into meaningful torque, angle, and current data:

    ```javascript
    const graph = inputData.params;
    let col2factor;
    let range;
    let sample_rate = 1000 / graph.sample_rate;

    switch (graph.col2factor) {
    // ... switch cases for col2factor
    }

    switch (graph.range) {
    // ... switch cases for range
    }

    // ... data extraction and transformation logic

    for (let i = 0; i < graph.cols; i++) {
        let curvData = [];
        // ... loop through graph data and process curves
        curveObjects.push(curveObject);
    }
    ```

    Parameters like `col2factor`, `range`, and `sample_rate` are extracted from the incoming graph data. Switch cases are employed to determine appropriate scaling factors.

    Inside the loop, the script iterates through each curve in the graph data, processing torque, angle, and current values based on the curve type.

4. **Publication to MQTT Topics:**

    Once the data is processed, the script publishes the results to respective MQTT topics:

    ```javascript
    // Publish the processed data to respective MQTT topics
    const torqueData = JSON.stringify(curveObjects[0]);
    const angleData = JSON.stringify(curveObjects[1]);
    const currentData = JSON.stringify(curveObjects[2]);

    client.publish('BTC/notifications/BTC.graphtorque', torqueData);
    client.publish('BTC/notifications/BTC.graphangle', angleData);
    client.publish('BTC/notifications/BTC.graphcurrent', currentData);
    ```

    The processed torque, angle, and current data are converted to JSON strings and published to their corresponding MQTT topics.

    Feel free to explore and customize this data processing logic based on your specific graph data structure and processing requirements. Adjusting switch cases, scaling factors, or adding custom processing steps can tailor the script to your needs.

4. **Error Handling:**

   ```javascript
   client.on('error', (error) => {
     console.error('MQTT Error:', error);
     // Handle the error as needed, and optionally exit with an error code
     process.exit(1);
   });

   client.on('close', () => {
     console.log('MQTT Connection closed');
   });
   ```

   Log MQTT connection errors and close events, providing feedback in case of issues.

   Feel free to explore the script, customize MQTT connection parameters, and adapt the data processing logic based on your specific use case.
