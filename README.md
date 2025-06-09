# Good Deed Meter

A minimalist Android application that allows users to record visits to self-defined religious sites (mosque, synagogue, temple, church, etc.) as "good deeds." The app works entirely offline after installation, storing data locally and operating a low-overhead background location monitor based on Android Geofencing.

## 📱 Features

- **Location-Based Tracking**: Automatically records visits to user-defined religious sites using geofencing technology
- **Privacy-Focused**: Works completely offline with all data stored locally on your device
- **Minimal Battery Impact**: Low-overhead background monitoring with ≤ 2% battery drain per 24h
- **Visit History**: View your complete history of visits in a simple chronological list
- **Statistics Dashboard**: Track your progress with weekly, monthly, and yearly summaries
- **Data Export**: Export your visit history to CSV format for personal records

## 🔧 Requirements

- Android 8.0 (API 26) or higher
- Google Play Services (for Geofencing API)
- Location permissions enabled

## 📲 Installation

*Note: The app is currently in development and not yet available for installation.*

Once released, the app will be available through:
- Google Play Store
- Direct APK download from GitHub releases

## 🚀 Usage

*Note: The following features are currently in development and not yet available.*

### Adding a Religious Site

1. Navigate to the "Add Location" tab
2. Enter the address of the religious site
3. Select from the search results to confirm the location
4. Customize the name (optional) and save

### Enabling Background Tracking

1. Open the app and navigate to the Dashboard
2. Toggle the "Background Tracking" switch to ON
3. A persistent notification will appear, indicating tracking is active
4. When you visit a saved location, a "Good deed logged ✓" notification will appear

### Viewing Visit History

1. Navigate to the "History" tab
2. Browse your visits in reverse chronological order
3. Each entry shows the location name, date/time, and visit count for that day

### Exporting Data

1. Navigate to the "History" tab
2. Tap the export icon in the top menu
3. A CSV file will be saved to your Downloads folder

## 🏗️ Architecture

The app follows a clean architecture approach with:

```
UI Layer  ─┬─ DashboardFragment
          ├─ AddLocationFragment
          └─ HistoryFragment
Domain     ├─ VisitLogger (handles geofence callbacks)
Layer      └─ ExportManager
Data Layer ├─ SQLiteOpenHelper (schema v1)
          ├─ LocationDao  (raw SQL wrappers)
          └─ VisitDao
Background └─ GeofenceService : ForegroundService
```

## 🔒 Privacy

- **No Data Collection**: The app does not collect or transmit any personal data
- **No Analytics**: No analytics or tracking SDKs are included
- **Offline First**: All core features work without an internet connection after initial setup
- **Local Storage Only**: All data remains on your device

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the terms included in the [LICENSE](LICENSE) file.

## 🔮 Future Enhancements

The following features are planned for future releases:

- Dark theme support
- Location editing and deletion
- Cloud backup / cross-device synchronization
- Home screen widget showing weekly progress