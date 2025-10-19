# Technical App - Flutter

แอปพลิเคชันสำหรับช่างเทคนิคที่รับงานและให้บริการ

## โครงสร้างโปรเจค

```
technical-app/
├── android/                    # Android platform files
├── ios/                        # iOS platform files
├── lib/
│   ├── main.dart              # Entry point
│   ├── app.dart               # App widget
│   │
│   ├── core/                  # Core functionality
│   │   ├── constants/
│   │   ├── theme/
│   │   ├── utils/
│   │   └── config/
│   │
│   ├── data/                  # Data layer
│   │   ├── models/
│   │   │   ├── user_model.dart
│   │   │   ├── technician_profile_model.dart
│   │   │   ├── service_request_model.dart
│   │   │   ├── job_model.dart
│   │   │   └── earnings_model.dart
│   │   ├── repositories/
│   │   │   ├── auth_repository.dart
│   │   │   ├── job_repository.dart
│   │   │   ├── earnings_repository.dart
│   │   │   └── profile_repository.dart
│   │   ├── providers/
│   │   │   ├── api_provider.dart
│   │   │   ├── socket_provider.dart
│   │   │   └── location_provider.dart
│   │   └── local/
│   │       └── local_database.dart
│   │
│   ├── domain/                # Business logic layer
│   │   ├── entities/
│   │   ├── usecases/
│   │   │   ├── auth/
│   │   │   ├── job/
│   │   │   │   ├── get_available_jobs_usecase.dart
│   │   │   │   ├── accept_job_usecase.dart
│   │   │   │   ├── decline_job_usecase.dart
│   │   │   │   └── update_job_status_usecase.dart
│   │   │   ├── profile/
│   │   │   │   ├── update_availability_usecase.dart
│   │   │   │   └── update_profile_usecase.dart
│   │   │   └── earnings/
│   │   │       └── get_earnings_usecase.dart
│   │   └── repositories/
│   │
│   ├── presentation/          # UI layer
│   │   ├── screens/
│   │   │   ├── splash/
│   │   │   │   └── splash_screen.dart
│   │   │   ├── auth/
│   │   │   │   ├── login_screen.dart
│   │   │   │   ├── register_screen.dart
│   │   │   │   └── registration_form_screen.dart
│   │   │   ├── onboarding/
│   │   │   │   ├── onboarding_screen.dart
│   │   │   │   ├── specialization_screen.dart
│   │   │   │   ├── experience_screen.dart
│   │   │   │   ├── documents_upload_screen.dart
│   │   │   │   └── verification_pending_screen.dart
│   │   │   ├── home/
│   │   │   │   ├── home_screen.dart
│   │   │   │   └── widgets/
│   │   │   │       ├── availability_toggle.dart
│   │   │   │       ├── stats_card.dart
│   │   │   │       └── pending_jobs_card.dart
│   │   │   ├── jobs/
│   │   │   │   ├── available_jobs_screen.dart
│   │   │   │   ├── job_detail_screen.dart
│   │   │   │   ├── my_jobs_screen.dart
│   │   │   │   ├── job_history_screen.dart
│   │   │   │   └── widgets/
│   │   │   │       ├── job_card.dart
│   │   │   │       ├── job_filter.dart
│   │   │   │       └── distance_badge.dart
│   │   │   ├── navigation/
│   │   │   │   ├── navigation_screen.dart
│   │   │   │   └── widgets/
│   │   │   │       └── map_widget.dart
│   │   │   ├── work/
│   │   │   │   ├── work_detail_screen.dart
│   │   │   │   ├── quote_screen.dart
│   │   │   │   ├── work_progress_screen.dart
│   │   │   │   └── completion_screen.dart
│   │   │   ├── chat/
│   │   │   │   ├── chat_list_screen.dart
│   │   │   │   ├── chat_screen.dart
│   │   │   │   └── widgets/
│   │   │   │       └── message_bubble.dart
│   │   │   ├── earnings/
│   │   │   │   ├── earnings_screen.dart
│   │   │   │   ├── earnings_detail_screen.dart
│   │   │   │   ├── payout_history_screen.dart
│   │   │   │   └── widgets/
│   │   │   │       ├── earnings_chart.dart
│   │   │   │       └── payout_card.dart
│   │   │   ├── profile/
│   │   │   │   ├── profile_screen.dart
│   │   │   │   ├── edit_profile_screen.dart
│   │   │   │   ├── availability_settings_screen.dart
│   │   │   │   ├── certifications_screen.dart
│   │   │   │   ├── bank_account_screen.dart
│   │   │   │   └── settings_screen.dart
│   │   │   ├── reviews/
│   │   │   │   ├── reviews_screen.dart
│   │   │   │   └── review_detail_screen.dart
│   │   │   └── notifications/
│   │   │       └── notifications_screen.dart
│   │   │
│   │   ├── widgets/           # Shared widgets
│   │   │   ├── common/
│   │   │   ├── cards/
│   │   │   └── dialogs/
│   │   │
│   │   └── state/             # State management
│   │       ├── providers/
│   │       │   ├── auth_provider.dart
│   │       │   ├── job_provider.dart
│   │       │   ├── location_provider.dart
│   │       │   └── earnings_provider.dart
│   │       └── controllers/
│   │
│   └── services/              # Services
│       ├── firebase/
│       ├── location_service.dart
│       ├── notification_service.dart
│       └── background_service.dart
│
├── test/
├── assets/
├── pubspec.yaml
└── README.md
```

## Key Dependencies

```yaml
dependencies:
  flutter:
    sdk: flutter
  
  # State Management
  provider: ^6.1.1
  
  # Networking
  dio: ^5.4.0
  retrofit: ^4.0.3
  
  # WebSocket
  socket_io_client: ^2.0.3
  
  # Local Storage
  shared_preferences: ^2.2.2
  hive: ^2.2.3
  
  # Firebase
  firebase_core: ^2.24.2
  firebase_auth: ^4.15.3
  firebase_messaging: ^14.7.9
  
  # UI
  google_fonts: ^6.1.0
  flutter_svg: ^2.0.9
  cached_network_image: ^3.3.0
  
  # Maps & Location
  google_maps_flutter: ^2.5.0
  geolocator: ^10.1.0
  geocoding: ^2.1.1
  flutter_polyline_points: ^2.0.0
  
  # Background Tasks
  workmanager: ^0.5.1
  flutter_background_service: ^5.0.3
  
  # Camera & Images
  image_picker: ^1.0.5
  camera: ^0.10.5
  
  # Charts
  fl_chart: ^0.65.0
  
  # Utilities
  intl: ^0.18.1
  url_launcher: ^6.2.2
```

## Features

### 1. Registration & Onboarding
- ลงทะเบียนพร้อมข้อมูลช่าง
- เลือกความเชี่ยวชาญ
- ระบุประสบการณ์และใบรับรอง
- อัพโหลดเอกสาร (บัตรประชาชน, ใบรับรอง)
- ข้อมูลบัญชีธนาคาร
- รอการอนุมัติจาก Admin

### 2. Job Management
- ดูงานที่เปิดรับใกล้เคียง
- กรองงานตามหมวดหมู่และระยะทาง
- รับ/ปฏิเสธงาน
- ดูรายละเอียดงาน
- ประวัติงานที่ทำ

### 3. Work Flow
- Navigate ไปยังที่อยู่ลูกค้า
- แจ้งเมื่อถึงที่หมาย
- ประเมินและเสนอราคา
- อัพเดทสถานะงาน
- ถ่ายรูปผลงาน
- บันทึกค่าใช้จ่ายจริง
- แจ้งเสร็จงาน

### 4. Communication
- แชทกับลูกค้า
- ส่งรูปภาพและตำแหน่ง
- แจ้งเตือนข้อความใหม่

### 5. Earnings & Payouts
- ดูรายได้รายวัน/รายเดือน
- กราฟแสดงรายได้
- ประวัติการจ่ายเงิน
- รายละเอียดแต่ละงาน
- ค่าธรรมเนียมแพลตฟอร์ม

### 6. Profile Management
- แก้ไขโปรไฟล์
- จัดการความเชี่ยวชาญ
- อัพเดทใบรับรอง
- ตั้งค่าเวลาทำงาน
- กำหนดรัศมีการรับงาน
- เปิด/ปิดสถานะพร้อมรับงาน

### 7. Reviews & Ratings
- ดูรีวิวและคะแนน
- ตอบกลับรีวิว
- สถิติคะแนนเฉลี่ย

### 8. Availability Management
- สลับสถานะพร้อมรับงาน
- ตั้งค่าเวลาทำงาน
- ระบุรัศมีการให้บริการ
- Background location tracking (เมื่อเปิดรับงาน)

## Setup Instructions

### Prerequisites
- Flutter 3.0+
- Dart 3.0+
- Android Studio / Xcode
- Firebase Account
- Google Maps API Key

### Installation

1. Clone repository:
```bash
git clone https://github.com/your-repo/fixfast-technical-app.git
cd technical-app
```

2. Install dependencies:
```bash
flutter pub get
```

3. Configure Firebase:
```bash
flutterfire configure
```

4. Add Google Maps API Key:

**Android** (`android/app/src/main/AndroidManifest.xml`):
```xml
<meta-data
    android:name="com.google.android.geo.API_KEY"
    android:value="YOUR_GOOGLE_MAPS_API_KEY"/>
```

**iOS** (`ios/Runner/AppDelegate.swift`):
```swift
GMSServices.provideAPIKey("YOUR_GOOGLE_MAPS_API_KEY")
```

5. Configure environment:
```dart
// lib/core/config/env_config.dart
class EnvConfig {
  static const String apiBaseUrl = 'YOUR_API_URL';
  static const String socketUrl = 'YOUR_SOCKET_URL';
  static const String googleMapsApiKey = 'YOUR_GOOGLE_MAPS_KEY';
}
```

6. Run the app:
```bash
flutter run
```

## Key Features Implementation

### 1. Location Tracking

```dart
// services/location_service.dart
class LocationService {
  final Geolocator _geolocator = Geolocator();
  StreamSubscription<Position>? _positionSubscription;
  
  Future<void> startTracking(Function(Position) onLocationUpdate) async {
    bool serviceEnabled = await Geolocator.isLocationServiceEnabled();
    if (!serviceEnabled) {
      throw Exception('Location services are disabled');
    }
    
    LocationPermission permission = await Geolocator.checkPermission();
    if (permission == LocationPermission.denied) {
      permission = await Geolocator.requestPermission();
      if (permission == LocationPermission.denied) {
        throw Exception('Location permissions are denied');
      }
    }
    
    _positionSubscription = Geolocator.getPositionStream(
      locationSettings: LocationSettings(
        accuracy: LocationAccuracy.high,
        distanceFilter: 10, // Update every 10 meters
      ),
    ).listen((Position position) {
      onLocationUpdate(position);
    });
  }
  
  void stopTracking() {
    _positionSubscription?.cancel();
  }
  
  Future<double> calculateDistance(
    double startLat,
    double startLng,
    double endLat,
    double endLng,
  ) async {
    return Geolocator.distanceBetween(startLat, startLng, endLat, endLng) / 1000; // km
  }
}
```

### 2. Job Notification Handler

```dart
// services/notification_service.dart
class NotificationService {
  final FirebaseMessaging _fcm = FirebaseMessaging.instance;
  
  Future<void> initialize() async {
    // Request permission
    NotificationSettings settings = await _fcm.requestPermission(
      alert: true,
      badge: true,
      sound: true,
    );
    
    // Get FCM token
    String? token = await _fcm.getToken();
    print('FCM Token: $token');
    
    // Handle foreground messages
    FirebaseMessaging.onMessage.listen((RemoteMessage message) {
      print('Got a message whilst in the foreground!');
      _showLocalNotification(message);
    });
    
    // Handle background messages
    FirebaseMessaging.onMessageOpenedApp.listen((RemoteMessage message) {
      print('Message clicked!');
      _handleNotificationClick(message);
    });
  }
  
  void _handleNotificationClick(RemoteMessage message) {
    if (message.data['type'] == 'new_job') {
      // Navigate to job detail
      String jobId = message.data['jobId'];
      // Navigation logic here
    }
  }
}
```

### 3. Background Location Service

```dart
// services/background_service.dart
import 'package:flutter_background_service/flutter_background_service.dart';

class BackgroundLocationService {
  static Future<void> initializeService() async {
    final service = FlutterBackgroundService();
    
    await service.configure(
      androidConfiguration: AndroidConfiguration(
        onStart: onStart,
        autoStart: false,
        isForegroundMode: true,
      ),
      iosConfiguration: IosConfiguration(
        autoStart: false,
        onForeground: onStart,
        onBackground: onIosBackground,
      ),
    );
  }
  
  @pragma('vm:entry-point')
  static void onStart(ServiceInstance service) async {
    Timer.periodic(Duration(seconds: 30), (timer) async {
      if (service is AndroidServiceInstance) {
        if (await service.isForegroundService()) {
          // Get current location
          Position position = await Geolocator.getCurrentPosition();
          
          // Send to server
          // await ApiProvider().updateLocation(position);
          
          service.setForegroundNotificationInfo(
            title: "FixFast - Available",
            content: "You're ready to receive jobs",
          );
        }
      }
    });
  }
  
  @pragma('vm:entry-point')
  static Future<bool> onIosBackground(ServiceInstance service) async {
    return true;
  }
}
```

### 4. Job Acceptance Flow

```dart
// presentation/state/providers/job_provider.dart
class JobProvider extends ChangeNotifier {
  final JobRepository _repository;
  
  List<Job> _availableJobs = [];
  List<Job> _myJobs = [];
  bool _isLoading = false;
  
  Future<void> loadAvailableJobs(Position currentPosition) async {
    _isLoading = true;
    notifyListeners();
    
    try {
      _availableJobs = await _repository.getAvailableJobs(
        latitude: currentPosition.latitude,
        longitude: currentPosition.longitude,
        maxDistance: 15, // km
      );
      
      // Sort by distance
      _availableJobs.sort((a, b) => 
        a.distanceFromTechnician.compareTo(b.distanceFromTechnician)
      );
      
      _isLoading = false;
      notifyListeners();
    } catch (e) {
      _isLoading = false;
      notifyListeners();
      rethrow;
    }
  }
  
  Future<bool> acceptJob(String jobId) async {
    try {
      final job = await _repository.acceptJob(jobId);
      
      // Remove from available jobs
      _availableJobs.removeWhere((j) => j.id == jobId);
      
      // Add to my jobs
      _myJobs.insert(0, job);
      
      notifyListeners();
      return true;
    } catch (e) {
      // Handle error (job might be taken by another technician)
      if (e is JobAlreadyTakenException) {
        _availableJobs.removeWhere((j) => j.id == jobId);
        notifyListeners();
      }
      return false;
    }
  }
  
  Future<void> updateJobStatus(String jobId, JobStatus status) async {
    try {
      await _repository.updateJobStatus(jobId, status);
      
      // Update local state
      final index = _myJobs.indexWhere((j) => j.id == jobId);
      if (index != -1) {
        _myJobs[index] = _myJobs[index].copyWith(status: status);
        notifyListeners();
      }
    } catch (e) {
      rethrow;
    }
  }
}
```

### 5. Navigation to Customer

```dart
// presentation/screens/navigation/navigation_screen.dart
class NavigationScreen extends StatefulWidget {
  final Job job;
  
  const NavigationScreen({required this.job});
  
  @override
  _NavigationScreenState createState() => _NavigationScreenState();
}

class _NavigationScreenState extends State<NavigationScreen> {
  late GoogleMapController _mapController;
  Position? _currentPosition;
  List<LatLng> _polylineCoordinates = [];
  
  @override
  void initState() {
    super.initState();
    _getCurrentLocation();
    _getDirections();
  }
  
  Future<void> _getCurrentLocation() async {
    Position position = await Geolocator.getCurrentPosition();
    setState(() {
      _currentPosition = position;
    });
  }
  
  Future<void> _getDirections() async {
    if (_currentPosition == null) return;
    
    PolylinePoints polylinePoints = PolylinePoints();
    PolylineResult result = await polylinePoints.getRouteBetweenCoordinates(
      EnvConfig.googleMapsApiKey,
      PointLatLng(_currentPosition!.latitude, _currentPosition!.longitude),
      PointLatLng(widget.job.location.latitude, widget.job.location.longitude),
    );
    
    if (result.points.isNotEmpty) {
      _polylineCoordinates = result.points
          .map((point) => LatLng(point.latitude, point.longitude))
          .toList();
      setState(() {});
    }
  }
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: GoogleMap(
        initialCameraPosition: CameraPosition(
          target: LatLng(
            _currentPosition?.latitude ?? 0,
            _currentPosition?.longitude ?? 0,
          ),
          zoom: 14,
        ),
        markers: {
          // Current position marker
          Marker(
            markerId: MarkerId('current'),
            position: LatLng(
              _currentPosition?.latitude ?? 0,
              _currentPosition?.longitude ?? 0,
            ),
            icon: BitmapDescriptor.defaultMarkerWithHue(BitmapDescriptor.hueBlue),
          ),
          // Destination marker
          Marker(
            markerId: MarkerId('destination'),
            position: LatLng(
              widget.job.location.latitude,
              widget.job.location.longitude,
            ),
            icon: BitmapDescriptor.defaultMarkerWithHue(BitmapDescriptor.hueRed),
          ),
        },
        polylines: {
          Polyline(
            polylineId: PolylineId('route'),
            points: _polylineCoordinates,
            color: Colors.blue,
            width: 5,
          ),
        },
        onMapCreated: (controller) {
          _mapController = controller;
        },
      ),
      floatingActionButton: FloatingActionButton.extended(
        onPressed: () => _openGoogleMaps(),
        label: Text('Open Google Maps'),
        icon: Icon(Icons.directions),
      ),
    );
  }
  
  void _openGoogleMaps() async {
    final url = 'https://www.google.com/maps/dir/?api=1&destination='
        '${widget.job.location.latitude},${widget.job.location.longitude}';
    if (await canLaunch(url)) {
      await launch(url);
    }
  }
}
```

## Availability Management

```dart
// presentation/widgets/availability_toggle.dart
class AvailabilityToggle extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Consumer<TechnicianProvider>(
      builder: (context, provider, child) {
        return SwitchListTile(
          title: Text('Available for Jobs'),
          subtitle: Text(
            provider.isAvailable 
              ? 'You will receive job notifications'
              : 'You won\'t receive job notifications',
          ),
          value: provider.isAvailable,
          onChanged: (value) async {
            if (value) {
              // Check location permission
              bool hasPermission = await _checkLocationPermission();
              if (!hasPermission) {
                _showLocationPermissionDialog(context);
                return;
              }
              
              // Start background service
              await BackgroundLocationService.initializeService();
            } else {
              // Stop background service
              FlutterBackgroundService().invoke('stop');
            }
            
            await provider.updateAvailability(value);
          },
        );
      },
    );
  }
}
```

## Earnings Display

```dart
// presentation/screens/earnings/earnings_screen.dart
class EarningsScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Earnings')),
      body: Consumer<EarningsProvider>(
        builder: (context, provider, child) {
          return SingleChildScrollView(
            child: Column(
              children: [
                // Summary Card
                EarningsSummaryCard(
                  totalEarnings: provider.totalEarnings,
                  thisMonthEarnings: provider.thisMonthEarnings,
                  pendingPayout: provider.pendingPayout,
                ),
                
                // Chart
                EarningsChart(data: provider.earningsData),
                
                // Recent Transactions
                ListView.builder(
                  shrinkWrap: true,
                  physics: NeverScrollableScrollPhysics(),
                  itemCount: provider.recentTransactions.length,
                  itemBuilder: (context, index) {
                    return TransactionCard(
                      transaction: provider.recentTransactions[index],
                    );
                  },
                ),
              ],
            ),
          );
        },
      ),
    );
  }
}
```

## Testing

```bash
# Run all tests
flutter test

# Test specific features
flutter test test/job_provider_test.dart
flutter test test/location_service_test.dart
```

## Build & Release

### Android
```bash
flutter build apk --release
flutter build appbundle --release
```

### iOS
```bash
flutter build ios --release
```

## Best Practices

1. **Battery Optimization**: Minimize background location updates
2. **Offline Support**: Cache job data for offline access
3. **Real-time Updates**: Use WebSocket for instant notifications
4. **Error Handling**: Handle network errors gracefully
5. **User Experience**: Provide clear feedback on all actions
6. **Performance**: Optimize map rendering and list scrolling

## Troubleshooting

### Location Permission Issues
```dart
// Check and request permissions
LocationPermission permission = await Geolocator.checkPermission();
if (permission == LocationPermission.denied) {
  permission = await Geolocator.requestPermission();
}
```

### Background Service Not Working
```bash
# Android: Check battery optimization settings
# iOS: Enable Background Modes in Xcode
```

## Contributing

Please read [CONTRIBUTING.md](../CONTRIBUTING.md) for details on contributing to this project.

## License

This project is licensed under the MIT License - see the [LICENSE](../LICENSE) file for details.
