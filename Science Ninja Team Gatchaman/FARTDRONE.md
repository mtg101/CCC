# No Really - Fart Drone for Under £100
(plus a laptop to run age detection...)


Picopiw2: £8
https://thepihut.com/products/raspberry-pi-pico-2-w

Grove vision v2: £16
https://thepihut.com/products/grove-vision-ai-module-v2

Camera - £13
https://thepihut.com/products/mini-camera-module-for-raspberry-pi

Passive buzzer - £4
https://thepihut.com/products/m5stack-passive-buzzer-unit

Cheap (no-camera) drone: £27
https://www.amazon.co.uk/4DRC-Quadcopter-Batteries-Aeroplane-Beginners/dp/B08KP8YYX7/?th=1

+ batteries, wires, breadboard - max £10

= £78 - fart dron for under £100


Fart drone for under £100 

(ok needs laptop / PI server but - we have those lying around :)



--- notes


PicoPI2 is going to struggl to fun basic bxo-around-faces from a camera.

Old phones that did this used dedicated DSPs plsu classic CV. 

Use a smart camera sensor They handle the face detection on-board at 30+ FPS and simply send the (x, y) bounding-box coordinates to your Pico over I2C or UART.

Grove Vision AI V2 is just board - https://www.amazon.co.uk/Gvvsjgdbis-Grove-Ethos-U55-TensorFlow-Supported-Blue/dp/B0DW471P1L/
Bring-your-own CSI camera: Unlike all-in-one modules such as the DFRobot HuskyLens or the older Grove Vision AI V1, the V2 board separates the AI processor (Himax WiseEye2) from the sensor. It is designed to plug into any standard Raspberry Pi Camera Module (like an OV5647, IMX219, or IMX477) via a flat ribbon cable.

Or all-in-one like HuskyLens - https://www.amazon.co.uk/youyeetoo-HUSKYLENS-Machine-Vision-Sensor/dp/B086GP9PN2/
That's V1 - does basics but V2 better - Amazon not good for this - other pages or other smart camers?

V2 - £70 https://www.amazon.co.uk/DFRobot-Gravity-HUSKYLENS-Compatible-Raspberry/dp/B0HJ58GQGN/


(pihut is better for these... probably the other place too)

Oh also: you don't get the video - just the face bounding boxes. 

How each board handles single-frame capture

Grove Vision AI V2: Over a fast UART link (921,600 baud, which the Pico 2's hardware UART handles easily), you can send an AT command (AT+INVOKE=1,0,0) that returns both the face bounding box coordinates and the actual JPEG image frame (base64-encoded inside a JSON response). You can leave the image flag off during normal polling so you get fast coordinates, and only request the image payload the moment a face triggers your logic.


Needs OV5647 camera.


Original HuskyLens: Its UART/I2C protocol only transmits detection metadata (coordinates, box dimensions, and IDs). It has no command to stream or dump a frame over the cable to your Pico 2.

HuskyLens 2 (£70): Includes a microSD card slot for saving snapshots locally on the camera, and can stream images over Wi-Fi if you add the Wi-Fi daughterboard, though its 4-pin Pico link still focuses on coordinates and metadata.

HuskyLens 2: No built-in age model either—its face models cover Face Recognition (identifying specific learned people), Face Orientation (roll/yaw/pitch), and Face Emotion Recognition. However, because HuskyLens 2 lets you deploy custom-trained models via DFRobot's Mind+ / YOLO conversion workflow, you could theoretically train a custom classifier with two classes (kid vs. adult) on a dataset of faces and flash it onto the device.

Or send over wifi. Coudld be but but also:
Option A: Local laptop on the classroom Wi-Fi (Fastest & zero cloud API keys/privacy headaches). Run a 20-line Python FastAPI/Flask script on a laptop in the room using a lightweight library like DeepFace or InsightFace. Latency is ~200–500 ms, nothing leaves the room, and students can inspect the server code live.
(doesn't need gfx card for single image every few seconds)



Plus speaker
PWM + small speaker / piezo / PAM8302 amp: Good for bleeps, chiptune-style happy arpeggios, and synth-style "fart" sweeps—very easy to code and fun to inspect.

DFPlayer Mini (MP3 module over UART) or I2S DAC/amp: Best if you want pre-recorded genuinely funny sound effects (like a proper trombone/fart MP3 or happy fanfare) without eating Pico flash/RAM.





Picopiw2: £8
https://thepihut.com/products/raspberry-pi-pico-2-w

Grove vision v2: £16
https://thepihut.com/products/grove-vision-ai-module-v2

Camera - £13
https://thepihut.com/products/mini-camera-module-for-raspberry-pi

Passive buzzer - £4
https://thepihut.com/products/m5stack-passive-buzzer-unit

Cheap (no-camera) drone: £27
https://www.amazon.co.uk/4DRC-Quadcopter-Batteries-Aeroplane-Beginners/dp/B08KP8YYX7/?th=1

+ batteries, wires, breadboard - max £10

= £78 - fart dron for under £100


Fart drone for under £100 

(ok needs laptop / PI server but - we have those lying around :)



--- 
The laptop runs a tiny local web server (Flask or FastAPI) that receives the face JPEG from the Pico 2 W and uses a pre-trained facial-analysis model to estimate age—typically returning an age number or a kid vs. parent label.

How it works under the hood
Receive image: The laptop endpoint accepts a POST request containing the raw JPEG bytes (or base64 string) sent over your local network.

Find the face: A lightweight face detector locates the face box inside the frame so the age model isn't distracted by the background.

Estimate age & map to role: The age model outputs an estimated age (for example, 9 or 38). Your server applies a simple rule—such as age < 16 → "kid", age >= 16 → "parent"—and returns a small JSON response to the Pico 2 W.

Popular Python libraries for the laptop
InsightFace: Very fast on a laptop CPU (via ONNX Runtime) and handles both face detection and age/gender estimation cleanly in one pipeline.

DeepFace: Easy to get running quickly in Python and includes age prediction out of the box, though first-run model downloads take a moment and CPU speed depends on which detector backend you choose.
