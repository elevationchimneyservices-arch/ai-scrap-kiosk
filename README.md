# AI Scrap Kiosk

An AI-powered kiosk to automatically sort and recycle scrap materials. The kiosk combines computer vision, sensors, and a user facing interface to recognize and sort metal and other recyclables, rewarding users and collecting data for sustainability analytics.

## MVP feature list

- **Computer vision scrap classification** – a camera and vision model identify scrap items and categorize them (e.g., ferrous vs. non‑ferrous metals, steel grades). Studies show AI models like YOLO achieve ~94 % accuracy in distinguishing ferrous from non‑ferrous metals and can scan hundreds of items per minute ([AI, Blockchain & Drones Shaping Scrap Yards](https://www.cliftonmetals.com/2025/08/10/revolution-in-recycling-ai-blockchain-drones-shaping-scrap-yards/#:~:text=New%20technologies%20are%20giving%20scrap,moving%20ahead%20of%20the%20pack)). Models for steel‑scrap recognition detect types of scrap and quality, including hazardous objects ([A steel scrap recognition model based on machine vision](https://www.extrica.com/article/24321#:~:text=A%20steel%20scrap%20recognition%20model,the%20existence%20of%20dangerous%20goods)); aluminium‑scrap classifiers achieve ~98 % accuracy ([The Digital Kiosks of Tomorrow: How AI and Interactive ...](https://www.bridgewaterstudio.net/blog/digital-kiosks-of-tomorrow#:~:text=Computer%20Vision,for%20accurate%20commercial%20aluminum%20sorting)).
- **Automated sorting and bin monitoring** – servo‑controlled chutes direct items to the correct bins. Weight and volume sensors track fill levels, and the kiosk alerts staff when bins are full ([AI, Blockchain & Drones Shaping Scrap Yards](https://www.cliftonmetals.com/2025/08/10/revolution-in-recycling-ai-blockchain-drones-shaping-scrap-yards/#:~:text=It%E2%80%99s%20a%20great%20idea%20in,when%20the%20machine%20is%20full)).
- **User interface and reward system** – touch‑screen UI prompts the user to insert items and displays weight/earnings. A companion mobile app offers electronic cash‑out, coupons, rewards and environmental metrics similar to Olyns' system ([Olyns: AI-Powered Recycling & Retail Media](https://www.olyns.com/#:~:text=Olyns%3A%20AI,Powerful%20mobile%20app)).
- **Safety checks** – sensors and vision system detect hazardous objects (batteries, pressurised cans) to stop the conveyor and alert staff.
- **Data logging & analytics** – each transaction records material type, weight, and user ID. Data syncs to a back‑end for sustainability reporting and predictive maintenance.
- **MVP throughput** – design for ~40–60 items per minute using a moderate‑speed conveyor and vision model, inspired by recycling robots sorting 45 picks per minute ([Toward Fully Automated Metal Recycling using Computer ...](https://rl.cs.rutgers.edu/publications/HanCASE2021.pdf#:~:text=Penn%20Waste%20debuts%20AI%20recycling,of%2045%20picks%20per%20minute)).

## Data & workflow architecture

1. **Capture layer** – high‑resolution camera and LIDAR/weight sensors capture images and measurements of each item as it enters.
2. **Edge processing** – an embedded GPU board (e.g., NVIDIA Jetson Nano) runs the trained classification model. The model outputs material category and confidence.
3. **Sort & store** – a microcontroller triggers actuators to route items to the appropriate bin; weight sensors update fill‑level data.
4. **Application layer** – a Python/Flask (or FastAPI) service on the kiosk handles user interactions, aggregates sensor data and logs transactions.
5. **Back‑end & analytics** – data uploads periodically to a cloud platform (AWS/Azure) where a serverless API stores it in a database (PostgreSQL). Dashboards display volumes recycled, user participation and predictive maintenance.

## Hardware/Software stack

| Component            | Example hardware/software | Purpose |
|---|---|---|
| **Camera & sensors** | High‑res RGB camera; weight sensor; proximity/LIDAR sensor | Capture item images, weight and volume. |
| **Compute module**   | NVIDIA Jetson Nano or Xavier; Raspberry Pi as controller | Runs vision models and controls actuators. |
| **Actuators & bins** | Servo motors, solenoids, rotating chutes; load cells | Direct sorted items into bins; measure fill level. |
| **Touch UI**         | 10″ LCD touchscreen; simple kiosk enclosure | User inputs & displays transaction details. |
| **Software**         | Python 3; PyTorch/TensorFlow for models; Flask/FastAPI for API; SQLite/PostgreSQL; optional mobile app (Flutter/React Native) | Implement classification, control logic, UI, back‑end. |
| **Cloud services**   | AWS EC2/Lambda or Azure Functions; S3/Blob Storage; API Gateway | Data storage, remote monitoring, analytics. |

## Project timeline

| Phase | Duration | Key activities |
|---|---|---|
| **1 – Planning & research** | Weeks 1‑2 | Gather requirements; review literature and existing kiosks; plan dataset collection and hardware procurement. |
| **2 – Prototype vision model** | Weeks 3‑4 | Collect sample images; train a YOLO‑based classifier for metal categories; test accuracy on scrap dataset; refine to reach ~94–98 % accuracy ([AI, Blockchain & Drones Shaping Scrap Yards](https://www.cliftonmetals.com/2025/08/10/revolution-in-recycling-ai-blockchain-drones-shaping-scrap-yards/#:~:text=New%20technologies%20are%20giving%20scrap,moving%20ahead%20of%20the%20pack), [The Digital Kiosks of Tomorrow: How AI and Interactive ...](https://www.bridgewaterstudio.net/blog/digital-kiosks-of-tomorrow#:~:text=Computer%20Vision,for%20accurate%20commercial%20aluminum%20sorting)). |
| **3 – Hardware integration** | Weeks 5‑6 | Set up camera, sensors and Jetson board; integrate weight sensors; prototype sorting mechanism using servos. |
| **4 – Application & UI** | Weeks 7‑8 | Develop kiosk UI and simple mobile app; implement reward/points logic; integrate user authentication. |
| **5 – Data back‑end & analytics** | Weeks 9‑10 | Build cloud API; implement data upload, storage and dashboards; set up predictive maintenance alerts. |
| **6 – Testing & iteration** | Weeks 11‑12 | Conduct end‑to‑end tests; refine hardware & software; prepare for pilot deployment. |

## Starter GitHub repo

This repository contains initial scaffolding for the AI scrap kiosk:

- **scripts/data_collection.py** – placeholder script demonstrating how to capture images from a camera (integration with actual camera hardware required).
- **scripts/classifier_inference.py** – sample script that loads a machine‑learning model (e.g., PyTorch) and classifies an image; use this as a starting point for implementing your scrap‑classification model.

To run the sample scripts, clone the repository and install requirements:

```
git clone https://github.com/elevationchimneyservices-arch/ai-scrap-kiosk
cd ai-scrap-kiosk
python3 -m venv .venv
source .venv/bin/activate
pip install torch torchvision flask
python scripts/data_collection.py
python scripts/classifier_inference.py --image-path sample.jpg
```

Future development should expand these scripts into modules for model training, inference on the edge device, actuator control, UI, and cloud integration. Pull requests and issues are welcome.
