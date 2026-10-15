# T-24c Workouts

A free, open-source web app that controls the **adidas T-24c treadmill** over Bluetooth. It gives you structured speed and incline workouts, a Couch to 5K plan, GPX route runs and Strava upload, all from a single HTML page you can open in Chrome.


> I built this because the official adidas app stopped offering custom workouts that control speed and incline. It isn't made, endorsed or supported by adidas.

---

## Features

### 34 preset workouts
Seven types of workout, each with its own speed and incline profile:

| Focus | Workouts |
|---|---|
| **Recovery** | Recovery Jog, Recovery Walk, Shake-out & Strides |
| **Endurance** | Easy 30, Steady 45, Progression 40, Long Run 60, Long Run 90, Marathon Pace Blocks |
| **Speed** | Speed Intervals, 400m / 800m / 1K Repeats, Pyramid, Sprint Ladder, Leg Speed Strides |
| **Threshold** | Tempo Blocks, 20-Minute Tempo, Cruise Intervals, Over-Unders |
| **Hills** | Hill Repeats, Short Hill Sprints, Long Hill Reps, Hill Pyramid, Rolling Hills, Mountain Trail |
| **Fartlek** | Classic Fartlek, Minute On / Minute Off, 30/30 Surges, Hill Fartlek |
| **Walking** | Walk–Jog Builder, 12-3-30 Walk, Hill Walk Intervals, Long Climb |

A **Preset speed** slider (70–130%) scales every preset to your fitness. For example, at 114% Speed Intervals peak at 16 km/h instead of 14.

### Workout builder
- Make your own workouts from segments, each with a label, duration, speed and incline.
- Reorder, duplicate or delete segments.
- Add a whole block of intervals in one go.
- See a live chart of the workout with its total time, distance and climb.
- Copy and paste your workouts as text to back them up or move them to another device.

### Couch to 5K
A 19-session plan that goes from brisk walks to a continuous 5K, with no fixed timetable. Tick sessions off as you complete them, untick to redo one, and load any session straight onto the treadmill. A session ticks itself off when you finish it.

### Routes from GPX files
Upload a GPX route from Strava, Komoot, Garmin or similar, and the app turns its elevation profile into an incline workout based on distance.
- **Only the incline is controlled.** You choose the speed.
- You choose how often the incline changes (every 100–800 m).
- A **Gradient effect** setting (50–150%) softens or exaggerates the hills.
- Downhills run at 0%, and anything steeper than 12% is capped at 12%. The route page tells you how much of the course this affects.

### Run screen
- Large countdown and target speed and incline, with ±0.1 / ±0.5 / ±1 km/h and ±1% buttons to adjust on the fly.
- **Effort** control to scale the whole workout up or down mid-run.
- Live speed, incline, distance and pace read from the treadmill.
- Back and Skip buttons, a 3-2-1 start countdown, and beeps when a segment changes.
- **Free run** mode: clear the workout and control the treadmill by hand.
- Keeps the screen awake while you run.

### Strava upload
Every workout is recorded once a second (speed, incline, distance and climb) and can be uploaded to Strava as an indoor activity:
- each workout segment becomes its own lap
- the description includes distance, time, metres climbed, average and maximum incline, and a summary of the plan
- a **TCX download** is also available if you'd rather upload by hand

### Safety
- If you stop the belt on the console or pull the safety key, the workout pauses.
- If Bluetooth disconnects, the workout pauses.
- The treadmill's own Stop button and safety key always work, whatever the app is doing.

---

## Requirements

| | |
|---|---|
| **Treadmill** | adidas T-24c. Other treadmills that support the standard Bluetooth FTMS protocol may also work. |
| **Browser** | Chrome or Edge on **Android, Windows, macOS or ChromeOS**. Safari and Firefox don't support Web Bluetooth, so **iPhone and iPad aren't supported**. |
| **Connection** | The page must be served over **https** (GitHub Pages does this) or from `localhost`. |

---

## Getting started

The easiest way to get up and running (pun intended) is to download the html file to your phone, tablet or PC and open it in Chrome. Allow the bluetooth permissions and simply connect to the treadmill. Opening the file from Files on an Android tablet directly in Chrome seems to work without any issues.

### Use the hosted version
1. Open the app link at the top of this page in Chrome.
2. Close the adidas app, or turn off Bluetooth on the device running it. The treadmill accepts only one connection at a time.
3. Switch the treadmill on and clip in the safety key.
4. Tap **Connect treadmill** and pick the T-24c from the list.
5. Choose a workout and press **Start**.

On Android, Bluetooth and Location both need to be switched on before Chrome can find the treadmill.

You can try everything without a treadmill using the **Simulator** button.


## Connecting Strava (optional)

Each user connects with their own free Strava API application, so no shared keys or server are involved.

1. Go to **[strava.com/settings/api](https://www.strava.com/settings/api)** and create an application. Any name and website are fine.
2. Set **Authorization Callback Domain** to the domain you open the app from, for example `your-username.github.io` or `localhost`.
3. In the app, tap **Strava** in the header and paste in your app's **Client ID** and **Client Secret**.
4. Tap **Connect to Strava** and approve access.
5. After a workout, tap **Upload to Strava** in the "Workout recorded" panel.

Strava has no field for incline, so the incline details and total climb go in the activity description.

---

## Privacy and data

- **There's no server, account or tracking.** The app is a single static HTML file.
- Workouts, Couch to 5K progress, routes, settings and your Strava keys are stored **only in your browser** (`localStorage`). They don't sync between devices; use the workout backup text to move custom workouts.
- The only outside service the app ever contacts is Strava, and only when you choose to connect and upload.
- Clearing your browser's site data removes everything.

---

## How it works

The T-24c uses the standard **Bluetooth Fitness Machine Service (FTMS)**, the same protocol Zwift and Kinomap use. The app uses the browser's [Web Bluetooth API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Bluetooth_API) to:

- read live speed, incline and distance from the *Treadmill Data* characteristic (`0x2ACD`)
- send speed, incline, start, pause and stop commands through the *Fitness Machine Control Point* (`0x2AD9`)
- read the supported speed and incline ranges (`0x2AD4`, `0x2AD5`) so targets always stay within the machine's limits

Everything is in one `index.html` file with no build step and no dependencies apart from a Google Fonts stylesheet.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| The treadmill doesn't appear in the device list | Close the adidas app or any other connected app. On Android, turn on Bluetooth and Location. Make sure the treadmill is switched on. |
| "Bluetooth isn't available in this browser" | Use Chrome or Edge, served over https or `localhost`. iOS isn't supported. |
| It connects but speed and incline don't change | The app shows a "read-only" message. The treadmill isn't accepting remote control; reconnect after closing other apps. |
| The Strava redirect fails | Check that the Callback Domain in your Strava API settings matches the address you opened the app from. |
| The GPX file is rejected | The file needs elevation data. Export it again with elevation included. |

---

## Disclaimer

Exercise at your own risk. Use the safety key at all times, and check with a medical professional before starting a new exercise programme. This project is not affiliated with, endorsed by or connected to adidas, the treadmill's manufacturer or Strava. All trademarks belong to their owners.

## Licence

[MIT](LICENSE). Free to use, change and share.

## Contributing

Issues and pull requests are welcome, especially reports of other FTMS treadmills that work, and new preset workouts.
