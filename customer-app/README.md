# Customer App - Flutter

แอปพลิเคชันสำหรับลูกค้าที่ต้องการเรียกใช้บริการช่าง

## โครงสร้างโปรเจค

```
customer-app/
├── android/                    # Android platform files
├── ios/                        # iOS platform files
├── lib/
│   ├── main.dart              # Entry point
│   ├── app.dart               # App widget
│   │
│   ├── core/                  # Core functionality
│   │   ├── constants/         # App constants
│   │   │   ├── api_constants.dart
│   │   │   ├── app_colors.dart
│   │   │   ├── app_strings.dart
│   │   │   └── routes.dart
│   │   ├── theme/             # App theme
│   │   │   ├── app_theme.dart
│   │   │   └── text_styles.dart
│   │   ├── utils/             # Utility functions
│   │   │   ├── date_formatter.dart
│   │   │   ├── validators.dart
│   │   │   └── helpers.dart
│   │   └── config/            # App configuration
│   │       └── env_config.dart
│   │
│   ├── data/                  # Data layer
│   │   ├── models/            # Data models
│   │   │   ├── user_model.dart
│   │   │   ├── service_request_model.dart
│   │   │   ├── category_model.dart
│   │   │   ├── payment_model.dart
│   │   │   └── review_model.dart
│   │   ├── repositories/      # Data repositories
│   │   │   ├── auth_repository.dart
│   │   │   ├── request_repository.dart
│   │   │   ├── payment_repository.dart
│   │   │   └── review_repository.dart
│   │   ├── providers/         # API providers
│   │   │   ├── api_provider.dart
│   │   │   ├── socket_provider.dart
│   │   │   └── storage_provider.dart
│   │   └── local/             # Local storage
│   │       ├── local_database.dart
│   │       └── shared_prefs.dart
│   │
│   ├── domain/                # Business logic layer
│   │   ├── entities/          # Business entities
│   │   │   ├── user.dart
│   │   │   ├── service_request.dart
│   │   │   └── category.dart
│   │   ├── usecases/          # Use cases
│   │   │   ├── auth/
│   │   │   │   ├── login_usecase.dart
│   │   │   │   ├── register_usecase.dart
│   │   │   │   └── logout_usecase.dart
│   │   │   ├── request/
│   │   │   │   ├── create_request_usecase.dart
│   │   │   │   ├── get_requests_usecase.dart
│   │   │   │   └── cancel_request_usecase.dart
│   │   │   └── payment/
│   │   │       ├── create_payment_usecase.dart
│   │   │       └── confirm_payment_usecase.dart
│   │   └── repositories/      # Repository interfaces
│   │       ├── auth_repository_interface.dart
│   │       └── request_repository_interface.dart
│   │
│   ├── presentation/          # UI layer
│   │   ├── screens/           # App screens
│   │   │   ├── splash/
│   │   │   │   └── splash_screen.dart
│   │   │   ├── auth/
│   │   │   │   ├── login_screen.dart
│   │   │   │   ├── register_screen.dart
│   │   │   │   └── forgot_password_screen.dart
│   │   │   ├── home/
│   │   │   │   ├── home_screen.dart
│   │   │   │   └── widgets/
│   │   │   │       ├── category_card.dart
│   │   │   │       └── recent_request_card.dart
│   │   │   ├── request/
│   │   │   │   ├── create_request_screen.dart
│   │   │   │   ├── request_detail_screen.dart
│   │   │   │   ├── request_list_screen.dart
│   │   │   │   └── widgets/
│   │   │   │       ├── request_form.dart
│   │   │   │       ├── image_picker_widget.dart
│   │   │   │       └── location_picker.dart
│   │   │   ├── tracking/
│   │   │   │   ├── tracking_screen.dart
│   │   │   │   └── widgets/
│   │   │   │       ├── status_timeline.dart
│   │   │   │       └── technician_info.dart
│   │   │   ├── chat/
│   │   │   │   ├── chat_screen.dart
│   │   │   │   └── widgets/
│   │   │   │       ├── message_bubble.dart
│   │   │   │       └── message_input.dart
│   │   │   ├── payment/
│   │   │   │   ├── payment_screen.dart
│   │   │   │   ├── payment_method_screen.dart
│   │   │   │   └── payment_success_screen.dart
│   │   │   ├── review/
│   │   │   │   ├── review_screen.dart
│   │   │   │   └── widgets/
│   │   │   │       ├── rating_bar.dart
│   │   │   │       └── review_form.dart
│   │   │   ├── profile/
│   │   │   │   ├── profile_screen.dart
│   │   │   │   ├── edit_profile_screen.dart
│   │   │   │   └── settings_screen.dart
│   │   │   └── notifications/
│   │   │       └── notifications_screen.dart
│   │   │
│   │   ├── widgets/           # Shared widgets
│   │   │   ├── common/
│   │   │   │   ├── custom_button.dart
│   │   │   │   ├── custom_text_field.dart
│   │   │   │   ├── loading_indicator.dart
│   │   │   │   └── error_widget.dart
│   │   │   ├── cards/
│   │   │   │   ├── info_card.dart
│   │   │   │   └── summary_card.dart
│   │   │   └── dialogs/
│   │   │       ├── confirmation_dialog.dart
│   │   │       └── alert_dialog.dart
│   │   │
│   │   └── state/             # State management
│   │       ├── providers/
│   │       │   ├── auth_provider.dart
│   │       │   ├── request_provider.dart
│   │       │   ├── payment_provider.dart
│   │       │   └── theme_provider.dart
│   │       └── controllers/
│   │           ├── auth_controller.dart
│   │           └── request_controller.dart
│   │
│   └── services/              # Services
│       ├── firebase/
│       │   ├── firebase_auth_service.dart
│       │   ├── firebase_messaging_service.dart
│       │   └── firebase_storage_service.dart
│       ├── location_service.dart
│       ├── notification_service.dart
│       └── image_service.dart
│
├── test/                      # Unit & widget tests
│   ├── unit/
│   ├── widget/
│   └── integration/
│
├── assets/                    # Assets
│   ├── images/
│   ├── icons/
│   └── fonts/
│
├── pubspec.yaml              # Dependencies
└── README.md                 # This file
```

## Key Dependencies

```yaml
dependencies:
  flutter:
    sdk: flutter
  
  # State Management
  provider: ^6.1.1
  # หรือ riverpod: ^2.4.9
  
  # Networking
  dio: ^5.4.0
  retrofit: ^4.0.3
  pretty_dio_logger: ^1.3.1
  
  # WebSocket
  socket_io_client: ^2.0.3
  
  # Local Storage
  shared_preferences: ^2.2.2
  hive: ^2.2.3
  hive_flutter: ^1.1.0
  
  # Firebase
  firebase_core: ^2.24.2
  firebase_auth: ^4.15.3
  firebase_messaging: ^14.7.9
  firebase_storage: ^11.5.6
  
  # UI
  google_fonts: ^6.1.0
  flutter_svg: ^2.0.9
  cached_network_image: ^3.3.0
  shimmer: ^3.0.0
  
  # Maps & Location
  google_maps_flutter: ^2.5.0
  geolocator: ^10.1.0
  geocoding: ^2.1.1
  
  # Image Handling
  image_picker: ^1.0.5
  image_cropper: ^5.0.1
  
  # Payment
  stripe_flutter: ^9.5.0
  # หรือ omise_flutter: ^2.0.0
  
  # Utilities
  intl: ^0.18.1
  url_launcher: ^6.2.2
  path_provider: ^2.1.1
  
  # Code Generation
  json_annotation: ^4.8.1
  freezed_annotation: ^2.4.1

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^3.0.0
  build_runner: ^2.4.7
  json_serializable: ^6.7.1
  freezed: ^2.4.6
  mockito: ^5.4.4
```

## Features

### 1. Authentication
- สมัครสมาชิกด้วย Email/Phone
- เข้าสู่ระบบ
- Firebase Authentication
- Social Login (Google, Facebook)
- Biometric Authentication

### 2. Service Request
- เลือกประเภทบริการ
- กรอกรายละเอียด
- อัพโหลดรูปภาพ
- เลือกที่อยู่บนแผนที่
- กำหนดวันเวลา
- ระดับความเร่งด่วน

### 3. Request Tracking
- ติดตามสถานะแบบ real-time
- ดูข้อมูลช่าง
- แชทกับช่าง
- ดูตำแหน่งช่างบนแผนที่
- ประวัติการอัพเดทสถานะ

### 4. Payment
- เลือกวิธีการชำระเงิน
- Stripe/Omise integration
- รับใบเสร็จ
- ประวัติการชำระเงิน

### 5. Review & Rating
- ให้คะแนนช่าง (1-5 ดาว)
- เขียนรีวิว
- แนบรูปภาพ
- ดูรีวิวของช่าง

### 6. Profile Management
- แก้ไขโปรไฟล์
- จัดการที่อยู่
- ประวัติการใช้บริการ
- การตั้งค่า

### 7. Notifications
- Push notifications
- แจ้งเตือนสถานะงาน
- แจ้งเตือนข้อความใหม่
- แจ้งเตือนการชำระเงิน

## Setup Instructions

### Prerequisites
- Flutter 3.0+
- Dart 3.0+
- Android Studio / Xcode
- Firebase Account

### Installation

1. Clone repository:
```bash
git clone https://github.com/your-repo/fixfast-customer-app.git
cd customer-app
```

2. Install dependencies:
```bash
flutter pub get
```

3. Configure Firebase:
```bash
# Install FlutterFire CLI
dart pub global activate flutterfire_cli

# Configure Firebase
flutterfire configure
```

4. Create environment configuration:
```dart
// lib/core/config/env_config.dart
class EnvConfig {
  static const String apiBaseUrl = 'YOUR_API_URL';
  static const String socketUrl = 'YOUR_SOCKET_URL';
  static const String googleMapsApiKey = 'YOUR_GOOGLE_MAPS_KEY';
  static const String stripePublishableKey = 'YOUR_STRIPE_KEY';
}
```

5. Run the app:
```bash
flutter run
```

## Build & Release

### Android
```bash
# Generate keystore
keytool -genkey -v -keystore ~/key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key

# Build APK
flutter build apk --release

# Build App Bundle
flutter build appbundle --release
```

### iOS
```bash
# Build iOS
flutter build ios --release

# Or use Xcode for signing and distribution
```

## Testing

```bash
# Run all tests
flutter test

# Run with coverage
flutter test --coverage

# Run integration tests
flutter drive --target=test_driver/app.dart
```

## Code Generation

```bash
# Generate models
flutter pub run build_runner build --delete-conflicting-outputs

# Watch mode
flutter pub run build_runner watch
```

## State Management Pattern

### Provider Example

```dart
// providers/request_provider.dart
class RequestProvider extends ChangeNotifier {
  final RequestRepository _repository;
  
  List<ServiceRequest> _requests = [];
  bool _isLoading = false;
  String? _error;
  
  List<ServiceRequest> get requests => _requests;
  bool get isLoading => _isLoading;
  String? get error => _error;
  
  Future<void> createRequest(CreateRequestDto dto) async {
    _isLoading = true;
    _error = null;
    notifyListeners();
    
    try {
      final request = await _repository.createRequest(dto);
      _requests.insert(0, request);
      _isLoading = false;
      notifyListeners();
    } catch (e) {
      _error = e.toString();
      _isLoading = false;
      notifyListeners();
      rethrow;
    }
  }
  
  Future<void> loadRequests() async {
    _isLoading = true;
    notifyListeners();
    
    try {
      _requests = await _repository.getRequests();
      _isLoading = false;
      notifyListeners();
    } catch (e) {
      _error = e.toString();
      _isLoading = false;
      notifyListeners();
    }
  }
}

// Usage in widget
class RequestListScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Consumer<RequestProvider>(
      builder: (context, provider, child) {
        if (provider.isLoading) {
          return LoadingIndicator();
        }
        
        if (provider.error != null) {
          return ErrorWidget(error: provider.error!);
        }
        
        return ListView.builder(
          itemCount: provider.requests.length,
          itemBuilder: (context, index) {
            return RequestCard(request: provider.requests[index]);
          },
        );
      },
    );
  }
}
```

## API Integration

```dart
// data/providers/api_provider.dart
class ApiProvider {
  final Dio _dio;
  
  ApiProvider() : _dio = Dio(BaseOptions(
    baseUrl: EnvConfig.apiBaseUrl,
    connectTimeout: Duration(seconds: 30),
    receiveTimeout: Duration(seconds: 30),
  )) {
    _dio.interceptors.add(AuthInterceptor());
    _dio.interceptors.add(PrettyDioLogger());
  }
  
  Future<Response> get(String path, {Map<String, dynamic>? params}) async {
    try {
      return await _dio.get(path, queryParameters: params);
    } on DioException catch (e) {
      throw _handleError(e);
    }
  }
  
  Future<Response> post(String path, {dynamic data}) async {
    try {
      return await _dio.post(path, data: data);
    } on DioException catch (e) {
      throw _handleError(e);
    }
  }
  
  Exception _handleError(DioException error) {
    switch (error.type) {
      case DioExceptionType.connectionTimeout:
        return NetworkException('Connection timeout');
      case DioExceptionType.badResponse:
        return ApiException(error.response?.data['message'] ?? 'Server error');
      default:
        return NetworkException('Network error');
    }
  }
}
```

## Real-time Updates

```dart
// data/providers/socket_provider.dart
class SocketProvider {
  late IO.Socket _socket;
  
  void connect(String token) {
    _socket = IO.io(EnvConfig.socketUrl, <String, dynamic>{
      'transports': ['websocket'],
      'auth': {'token': token},
    });
    
    _socket.onConnect((_) {
      print('Socket connected');
    });
    
    _socket.on('status_update', (data) {
      // Handle status update
    });
    
    _socket.on('new_message', (data) {
      // Handle new message
    });
  }
  
  void joinRoom(String requestId) {
    _socket.emit('join_room', {'roomId': requestId});
  }
  
  void sendMessage(String message) {
    _socket.emit('send_message', {'message': message});
  }
  
  void disconnect() {
    _socket.disconnect();
  }
}
```

## Localization

```dart
// l10n/app_en.arb
{
  "appName": "FixFast",
  "login": "Login",
  "register": "Register",
  "createRequest": "Create Request",
  "selectCategory": "Select Category"
}

// l10n/app_th.arb
{
  "appName": "ฟิกซ์ฟาสต์",
  "login": "เข้าสู่ระบบ",
  "register": "สมัครสมาชิก",
  "createRequest": "สร้างคำขอ",
  "selectCategory": "เลือกประเภทบริการ"
}

// pubspec.yaml
flutter:
  generate: true

// l10n.yaml
arb-dir: lib/l10n
template-arb-file: app_en.arb
output-localization-file: app_localizations.dart
```

## Best Practices

1. **Clean Architecture**: แยก layers ชัดเจน (presentation, domain, data)
2. **Dependency Injection**: ใช้ GetIt หรือ Provider สำหรับ DI
3. **Error Handling**: จัดการ error อย่างเหมาะสม
4. **Testing**: เขียน unit tests และ widget tests
5. **Code Generation**: ใช้ freezed และ json_serializable
6. **Null Safety**: ใช้ null safety features ของ Dart
7. **Performance**: ใช้ const constructors, lazy loading
8. **Accessibility**: รองรับ screen readers และ accessibility features

## Troubleshooting

### Common Issues

1. **Firebase Configuration Issues**
```bash
flutterfire configure
```

2. **Build Errors**
```bash
flutter clean
flutter pub get
flutter pub run build_runner build --delete-conflicting-outputs
```

3. **iOS Pods Issues**
```bash
cd ios
pod deintegrate
pod install
```

## Contributing

Please read [CONTRIBUTING.md](../CONTRIBUTING.md) for details on our code of conduct and the process for submitting pull requests.

## License

This project is licensed under the MIT License - see the [LICENSE](../LICENSE) file for details.
