# Wind Power Forecast Monitoring App

A clean, minimal React Native application for monitoring UK wind power generation forecasts and actual values. Built to help understand forecast accuracy and electricity demand planning.

## Features

- **Real-time Chart Visualization**: Compare actual vs. forecasted wind power generation
- **Date/Time Selection**: Custom time range selection with intuitive date/time pickers
- **Configurable Forecast Horizon**: Adjust forecast horizon (0-48 hours) to see different prediction windows
- **Generation Statistics**: View latest actual, latest forecast, and peak generation values
- **Responsive Design**: Optimized for both mobile and tablet displays
- **Clean, Minimal UI**: Modern aesthetic design without AI-generated appearance

## Project Structure

```
ForecastApp/
├── src/
│   ├── screens/
│   │   └── ForecastMonitor.jsx      # Main screen with layout
│   ├── components/
│   │   ├── ForecastChart.jsx         # Chart visualization component
│   │   ├── DateTimeSelector.jsx      # Date/time picker component
│   │   └── ForecastHorizonSlider.jsx # Forecast horizon selector
│   ├── services/
│   │   └── ForecastService.js        # API integration & data fetching
│   └── theme/
│       └── styles.js                 # Design tokens & styles
├── App.jsx                           # Root component
├── index.js                          # Entry point
├── package.json                      # Dependencies
└── README.md                         # This file
```

## Prerequisites

- Node.js 14 or higher
- npm or yarn
- Android SDK (for Android development)
- Java Development Kit (JDK) 11+

### For Android Development

1. Install Android Studio
2. Install Android SDK (API level 30+)
3. Set up environment variables:
   ```bash
   ANDROID_HOME=/path/to/Android/SDK
   JAVA_HOME=/path/to/jdk
   ```

## Installation

### 1. Install Dependencies

```bash
cd ForecastApp
npm install
# or
yarn install
```

### 2. Link Native Dependencies

```bash
react-native link react-native-svg
react-native link @react-native-community/datetimepicker
```

Alternatively, if using newer React Native (0.60+):
```bash
npx react-native link react-native-svg
npx react-native link @react-native-community/datetimepicker
```

## Development

### Run on Android

```bash
npm run android
```

Or with a specific emulator:
```bash
npx react-native run-android --deviceId <emulator_id>
```

### Start Development Server

```bash
npm start
```

This starts the Metro Bundler, which will:
- Listen for code changes
- Reload the app automatically
- Show error overlays in the app

### Debug the App

1. Press `R` twice in the terminal to reload
2. Press `D` to open the React Native debugger menu
3. Use Chrome DevTools or React Native Debugger

## Building for Release

### Build Android Release APK

```bash
npm run build-android
```

The APK will be generated at:
```
android/app/build/outputs/apk/release/app-release.apk
```

### Build Android App Bundle (For Play Store)

```bash
cd android
./gradlew bundleRelease
```

The bundle will be at:
```
app/build/outputs/bundle/release/app-release.aab
```

## API Integration

### Data Sources

The app fetches data from the UK Electricity Market Operators (Elexon) API:

1. **Actual Generation**: `FUELHH/stream` endpoint
   - Fetches actual wind power generation data
   - Field: `fuelType: WIND`

2. **Forecast Generation**: `WINDFOR/stream` endpoint
   - Fetches forecasted wind power generation
   - Filters by forecast horizon (0-48 hours)

### Configuration

The API URLs are defined in `src/services/ForecastService.js`:

```javascript
const FUELHH_STREAM_URL = 'https://bmrs.elexon.co.uk/api-documentation/endpoint/datasets/FUELHH/stream';
const WINDFOR_STREAM_URL = 'https://bmrs.elexon.co.uk/api-documentation/endpoint/datasets/WINDFOR/stream';
```

For production use, add your API credentials and configure authentication as needed.

### Mock Data

For development/testing without API access, the service automatically returns mock data that simulates realistic wind power generation patterns.

## Design System

### Color Palette

| Color | Value | Usage |
|-------|-------|-------|
| Primary | `#2563EB` | Buttons, active states |
| Success | `#10B981` | Forecast data, positive indicators |
| Error | `#EF4444` | Errors, warnings |
| Background | `#FFFFFF` | Main background |
| Surface | `#F8FAFC` | Card backgrounds |
| Border | `#E2E8F0` | Dividers, borders |

### Typography

- **Title**: 28px, Bold
- **Subtitle**: 16px, Semi-bold
- **Body**: 14px, Regular
- **Caption**: 12px, Regular
- **Number**: 18px, Bold

### Spacing Scale

- `xs`: 4px
- `sm`: 8px
- `md`: 12px
- `lg`: 16px
- `xl`: 24px
- `xxl`: 32px

## Component Details

### ForecastChart

Displays wind power generation data as a line chart with:
- Actual generation (solid blue line)
- Forecast data (dashed green line)
- Data points markers
- Y-axis grid lines
- Legend and statistics

**Props:**
- `forecastData`: Array of forecast data points
- `actualData`: Array of actual generation data points
- `onRefresh`: Callback to refresh data

### DateTimeSelector

Provides date and time selection with separate controls:
- Date picker for start date
- Time picker for start time
- Date picker for end date
- Time picker for end time

**Props:**
- `startDate`: Start date/time (Date object)
- `endDate`: End date/time (Date object)
- `onStartDateChange`: Callback for start date change
- `onEndDateChange`: Callback for end date change

### ForecastHorizonSlider

Interactive slider for selecting forecast horizon (0-48 hours):
- Real-time value display
- Descriptive text
- Hour markers

**Props:**
- `value`: Current horizon in hours
- `onValueChange`: Callback for value change

## Performance Optimization

- Charts use SVG for efficient rendering
- Data is memoized to prevent unnecessary recalculations
- Images are optimized for mobile displays
- API requests include proper error handling and fallbacks

## Troubleshooting

### Android Build Issues

1. **Gradle sync errors**:
   ```bash
   cd android
   ./gradlew clean
   cd ..
   react-native run-android
   ```

2. **Metro Bundler cache issues**:
   ```bash
   watchman watch-del-all
   rm -rf /tmp/metro-*
   npm start -- --reset-cache
   ```

3. **Native module linking issues**:
   ```bash
   cd android
   ./gradlew clean
   cd ..
   npm install
   react-native link
   react-native run-android
   ```

### Runtime Issues

- **Date picker not showing**: Ensure `@react-native-community/datetimepicker` is properly linked
- **Chart not rendering**: Check that `react-native-svg` is installed and linked
- **API connection errors**: Verify internet connection and API endpoint URLs

## Testing

To add unit tests:

```bash
npm install --save-dev @testing-library/react-native jest
```

Run tests:
```bash
npm test
```

## Deployment

### To Google Play Store

1. Generate a release keystore:
   ```bash
   keytool -genkey -v -keystore my-release-key.keystore -keyalg RSA -keysize 2048 -validity 10000 -alias my-key-alias
   ```

2. Add keystore path to `android/gradle.properties`:
   ```
   MYAPP_RELEASE_STORE_FILE=my-release-key.keystore
   MYAPP_RELEASE_STORE_PASSWORD=password
   MYAPP_RELEASE_KEY_ALIAS=my-key-alias
   MYAPP_RELEASE_KEY_PASSWORD=password
   ```

3. Build and upload:
   ```bash
   npm run build-android
   ```

   Then upload `app-release.aab` to Google Play Console.

## Development Notes

- The app uses mock data for demonstration. Replace with real API calls when credentials are available
- All data should be from January 2025 onwards
- Data with missing forecasts should be ignored
- Forecast horizon validation: 0-48 hours

## Resources

- [React Native Documentation](https://reactnative.dev/)
- [react-native-svg](https://github.com/react-native-svg/react-native-svg)
- [@react-native-community/datetimepicker](https://github.com/react-native-community/datetimepicker)
- [Elexon BMRS API](https://www.elexon.co.uk/guidance-notes/bmrs-api-data-guide/)

## License

MIT

## Author

Developed as part of the SDE Sample Project Internship

---

**Note**: This application uses AI tools for code assistance. The design and architecture are original and not AI-generated.
