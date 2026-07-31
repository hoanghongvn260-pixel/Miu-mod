import 'package:flutter/material.dart';
import 'package:url_launcher/url_launcher.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'App Kiem Tien',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: const HomeScreen(),
      debugShowCheckedModeBanner: false,
    );
  }
}

class HomeScreen extends StatefulWidget {
  const HomeScreen({super.key});

  @override
  State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  int _balance = 0; // Số xu hiện tại

  // Hàm mở link nhiệm vụ (vượt link)
  Future<void> _openTaskLink(String urlString) async {
    final Uri url = Uri.parse(urlString);
    if (!await launchUrl(url, mode: LaunchMode.externalApplication)) {
      throw Exception('Không thể mở liên kết $url');
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Ứng Dụng Kiếm Thưởng'),
        backgroundColor: Colors.blueAccent,
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Thẻ hiển thị số dư
            Container(
              padding: const EdgeInsets.all(20),
              decoration: BoxDecoration(
                color: Colors.blue.shade50,
                borderRadius: BorderRadius.circular(12),
                border: Border.all(color: Colors.blue.shade200),
              ),
              child: Row(
                mainAxisAlignment: MainAxisAlignment.spaceBetween,
                children: [
                  const Text('Số dư của bạn:', style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),
                  Text('$_balance Xu', style: const TextStyle(fontSize: 22, fontWeight: FontWeight.bold, color: Colors.green)),
                ],
              ),
            ),
            const SizedBox(height: 24),
            const Text('Danh sách nhiệm vụ:', style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),
            const SizedBox(height: 12),
            
            // Danh sách nhiệm vụ mẫu
            Expanded(
              child: ListView(
                children: [
                  TaskCard(
                    title: 'Nhiệm vụ 1: Vượt link nhận 500 xu',
                    reward: '+500 Xu',
                    onTap: () {
                      // Thay link bên dưới bằng link trang web vượt link của bạn ở Phần 2
                      _openTaskLink('https://example.com/vuotlink.php');
                      // Lưu ý: Thực tế cần cơ chế API gọi về server để cộng xu sau khi hoàn thành
                      setState(() {
                        _balance += 500; 
                      });
                    },
                  ),
                  TaskCard(
                    title: 'Nhiệm vụ 2: Xem quảng cáo banner',
                    reward: '+200 Xu',
                    onTap: () {
                      _openTaskLink('https://example.com/ads.php');
                      setState(() {
                        _balance += 200;
                      });
                    },
                  ),
                ],
              ),
            ),
          ],
        ),
      ),
    );
  }
}

class TaskCard extends StatelessWidget {
  final String title;
  final String reward;
  final VoidCallback onTap;

  const TaskCard({super.key, required this.title, required this.reward, required this.onTap});

  @override
  Widget build(BuildContext context) {
    return Card(
      elevation: 3,
      margin: const EdgeInsets.only(bottom: 12),
      child: ListTile(
        title: Text(title, style: const TextStyle(fontWeight: FontWeight.w500)),
        subtitle: Text(reward, style: const TextStyle(color: Colors.green, fontWeight: FontWeight.bold)),
        trailing: ElevatedButton(
          onPressed: onTap,
          child: const Text('Làm ngay'),
        ),
      ),
    );
  }
}
