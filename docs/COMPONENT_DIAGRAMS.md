# WireOS Victor Component Interaction Diagram

## System Component Relationships

```
┌─────────────────────────────────────────────────────────────────┐
│                        APPLICATION LAYER                         │
├─────────────────────────────────────────────────────────────────┤
│  SDK Interface  │  Web Server  │  Cloud Services  │  Mobile App  │
└─────────────────┴──────────────┴─────────────────┴─────────────────┘
                                   │
                              ┌────▼────┐
                              │ Engine  │
                              │ Layer   │
                              └────┬────┘
┌─────────────────────────────────────────────────────────────────┐
│                          ENGINE LAYER                           │
├─────────────────┬─────────────────┬─────────────────┬───────────┤
│   Behaviors     │    Vision       │   Navigation    │   Audio   │
│   - Actions     │   - Camera      │   - Path Plan   │   - TTS   │
│   - Animations  │   - Objects     │   - Mapping     │   - STT   │
│   - Emotions    │   - Faces       │   - Localize    │   - Sound │
└─────────────────┴─────────────────┴─────────────────┴───────────┘
                                   │
                              ┌────▼────┐
                              │ Robot   │
                              │ Layer   │
                              └────┬────┘
┌─────────────────────────────────────────────────────────────────┐
│                          ROBOT LAYER                            │
├─────────────────┬─────────────────┬─────────────────┬───────────┤
│   Hardware      │   Supervisor    │   Core Systems  │ Syscon    │
│   - Motors      │   - Monitoring  │   - Clock       │ - STM32   │
│   - Sensors     │   - Health      │   - Memory      │ - Power   │
│   - Display     │   - Recovery    │   - Storage     │ - Safety  │
└─────────────────┴─────────────────┴─────────────────┴───────────┘
                                   │
                              ┌────▼────┐
                              │Hardware │
                              │Physical │
                              └─────────┘

```

## Data Flow Architecture

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│ Sensors  │───▶│  Robot   │───▶│  Engine  │───▶│ Actions  │
│          │    │  Layer   │    │  Layer   │    │          │
└──────────┘    └──────────┘    └──────────┘    └──────────┘
     │               │               │               │
     ▼               ▼               ▼               ▼
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│Raw Data  │    │Processed │    │ State    │    │Commands  │
│- Camera  │    │- Vision  │    │- World   │    │- Motor   │
│- IMU     │    │- Audio   │    │- Robot   │    │- Display │
│- Touch   │    │- Motion  │    │- Intent  │    │- Audio   │
└──────────┘    └──────────┘    └──────────┘    └──────────┘

```

## Message Flow Between Components

```
    App/SDK                 Engine                    Robot
       │                      │                        │
       │──── User Intent ────▶│                        │
       │                      │                        │
       │                      │─── Sensor Request ───▶│
       │                      │◄── Sensor Data ───────│
       │                      │                        │
       │                      │─── State Update ─────▶│
       │                      │◄── Hardware Status ───│
       │                      │                        │
       │◄─── Robot Status ────│                        │
       │                      │                        │
       │◄─── Event Stream ────│                        │

```

## Thread and Process Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        Main Process                             │
├─────────────────┬─────────────────┬─────────────────┬───────────┤
│   Main Thread   │  Vision Thread  │  Audio Thread   │ IO Thread │
│   - Behaviors   │  - Camera       │  - Microphones  │ - Network │
│   - Logic       │  - Processing   │  - Speakers     │ - Storage │
│   - Coordination│  - Recognition  │  - Voice Proc   │ - Logging │
└─────────────────┴─────────────────┴─────────────────┴───────────┘
         │
    ┌────▼────┐
    │Robot    │
    │Process  │ ◄─── IPC Messages
    └─────────┘

```