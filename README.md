import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Kişilik Anketi',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        visualDensity: VisualDensity.adaptivePlatformDensity,
      ),
      home: PersonalitySurveyPage(),
    );
  }
}

// Sonuç Sayfası
class SurveyResultPage extends StatelessWidget {
  final String name;
  final String surname;
  final String gender;
  final List<String> hobbies;
  final bool smokingStatus;
  final int cigarettesPerDay;

  const SurveyResultPage({
    Key? key,
    required this.name,
    required this.surname,
    required this.gender,
    required this.hobbies,
    required this.smokingStatus,
    required this.cigarettesPerDay,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            begin: Alignment.topLeft,
            end: Alignment.bottomRight,
            colors: [
              Colors.purple[400]!,
              Colors.blue[600]!,
              Colors.teal[400]!,
            ],
          ),
        ),
        child: SafeArea(
          child: SingleChildScrollView(
            padding: EdgeInsets.all(20),
            child: Column(
              children: [
                // Header
                _buildHeader(context),
                SizedBox(height: 30),
                
                // Ana kart
                Container(
                  width: double.infinity,
                  decoration: BoxDecoration(
                    color: Colors.white,
                    borderRadius: BorderRadius.circular(20),
                    boxShadow: [
                      BoxShadow(
                        color: Colors.black.withOpacity(0.1),
                        blurRadius: 20,
                        offset: Offset(0, 10),
                      ),
                    ],
                  ),
                  child: Column(
                    children: [
                      // Profil bölümü
                      _buildProfileSection(),
                      
                      // Bilgiler bölümü
                      _buildInfoSection(),
                      
                      // Hobiler bölümü
                      _buildHobbiesSection(),
                      
                      // Sigara bölümü
                      _buildSmokingSection(),
                      
                      // Butonlar
                      _buildActionButtons(context),
                    ],
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }

  Widget _buildHeader(BuildContext context) {
    return Row(
      children: [
        IconButton(
          onPressed: () => Navigator.pop(context),
          icon: Icon(
            Icons.arrow_back_ios,
            color: Colors.white,
            size: 28,
          ),
        ),
        Expanded(
          child: Text(
            'Anket Sonuçları',
            style: TextStyle(
              color: Colors.white,
              fontSize: 24,
              fontWeight: FontWeight.bold,
            ),
            textAlign: TextAlign.center,
          ),
        ),
        SizedBox(width: 48), // Balance için
      ],
    );
  }

  Widget _buildProfileSection() {
    return Container(
      padding: EdgeInsets.all(25),
      decoration: BoxDecoration(
        gradient: LinearGradient(
          colors: [Colors.blue[50]!, Colors.purple[50]!],
        ),
        borderRadius: BorderRadius.only(
          topLeft: Radius.circular(20),
          topRight: Radius.circular(20),
        ),
      ),
      child: Column(
        children: [
          Container(
            width: 80,
            height: 80,
            decoration: BoxDecoration(
              shape: BoxShape.circle,
              gradient: LinearGradient(
                colors: [Colors.purple[400]!, Colors.blue[500]!],
              ),
            ),
            child: Icon(
              Icons.person,
              size: 40,
              color: Colors.white,
            ),
          ),
          SizedBox(height: 15),
          Text(
            '$name $surname',
            style: TextStyle(
              fontSize: 22,
              fontWeight: FontWeight.bold,
              color: Colors.grey[800],
            ),
          ),
          SizedBox(height: 5),
          Container(
            padding: EdgeInsets.symmetric(horizontal: 12, vertical: 6),
            decoration: BoxDecoration(
              color: Colors.blue[100],
              borderRadius: BorderRadius.circular(15),
            ),
            child: Text(
              gender,
              style: TextStyle(
                color: Colors.blue[800],
                fontWeight: FontWeight.w500,
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildInfoSection() {
    return Padding(
      padding: EdgeInsets.all(20),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          _buildSectionTitle('Kişisel Bilgiler', Icons.info_outline),
          SizedBox(height: 15),
          _buildInfoRow('Ad', name, Icons.person_outline, Colors.blue),
          _buildInfoRow('Soyad', surname, Icons.person_outline, Colors.green),
          _buildInfoRow('Cinsiyet', gender, Icons.wc, Colors.purple),
        ],
      ),
    );
  }

  Widget _buildHobbiesSection() {
    return Padding(
      padding: EdgeInsets.symmetric(horizontal: 20),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          _buildSectionTitle('Hobiler', Icons.favorite_outline),
          SizedBox(height: 15),
          if (hobbies.isEmpty)
            Container(
              padding: EdgeInsets.all(15),
              decoration: BoxDecoration(
                color: Colors.grey[100],
                borderRadius: BorderRadius.circular(10),
              ),
              child: Text(
                'Hiç hobi seçilmemiş',
                style: TextStyle(
                  color: Colors.grey[600],
                  fontStyle: FontStyle.italic,
                ),
              ),
            )
          else
            Wrap(
              spacing: 10,
              runSpacing: 10,
              children: hobbies.map((hobby) => _buildHobbyChip(hobby)).toList(),
            ),
          SizedBox(height: 20),
        ],
      ),
    );
  }

  Widget _buildSmokingSection() {
    return Container(
      margin: EdgeInsets.symmetric(horizontal: 20),
      padding: EdgeInsets.all(20),
      decoration: BoxDecoration(
        color: smokingStatus ? Colors.red[50] : Colors.green[50],
        borderRadius: BorderRadius.circular(15),
        border: Border.all(
          color: smokingStatus ? Colors.red[200]! : Colors.green[200]!,
        ),
      ),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Row(
            children: [
              Icon(
                smokingStatus ? Icons.smoking_rooms : Icons.smoke_free,
                color: smokingStatus ? Colors.red[600] : Colors.green[600],
              ),
              SizedBox(width: 10),
              Text(
                'Sigara Kullanımı',
                style: TextStyle(
                  fontSize: 18,
                  fontWeight: FontWeight.bold,
                  color: smokingStatus ? Colors.red[800] : Colors.green[800],
                ),
              ),
            ],
          ),
          SizedBox(height: 10),
          Container(
            padding: EdgeInsets.symmetric(horizontal: 12, vertical: 8),
            decoration: BoxDecoration(
              color: smokingStatus ? Colors.red[100] : Colors.green[100],
              borderRadius: BorderRadius.circular(8),
            ),
            child: Text(
              smokingStatus ? 'Evet, sigara kullanıyorum' : 'Hayır, sigara kullanmıyorum',
              style: TextStyle(
                fontWeight: FontWeight.w500,
                color: smokingStatus ? Colors.red[800] : Colors.green[800],
              ),
            ),
          ),
          if (smokingStatus && cigarettesPerDay > 0) ...[
            SizedBox(height: 10),
            Row(
              children: [
                Icon(Icons.linear_scale, color: Colors.red[600], size: 20),
                SizedBox(width: 8),
                Text(
                  'Günlük: $cigarettesPerDay adet',
                  style: TextStyle(
                    fontSize: 16,
                    fontWeight: FontWeight.w500,
                    color: Colors.red[700],
                  ),
                ),
              ],
            ),
          ],
        ],
      ),
    );
  }

  Widget _buildActionButtons(BuildContext context) {
    return Padding(
      padding: EdgeInsets.all(25),
      child: Column(
        children: [
          SizedBox(
            width: double.infinity,
            height: 50,
            child: ElevatedButton.icon(
              onPressed: () {
                Navigator.pop(context);
              },
              icon: Icon(Icons.edit),
              label: Text('Bilgileri Düzenle'),
              style: ElevatedButton.styleFrom(
                backgroundColor: Colors.blue[600],
                foregroundColor: Colors.white,
                shape: RoundedRectangleBorder(
                  borderRadius: BorderRadius.circular(25),
                ),
              ),
            ),
          ),
          SizedBox(height: 10),
          SizedBox(
            width: double.infinity,
            height: 50,
            child: OutlinedButton.icon(
              onPressed: () {
                _showShareDialog(context);
              },
              icon: Icon(Icons.share),
              label: Text('Sonuçları Paylaş'),
              style: OutlinedButton.styleFrom(
                foregroundColor: Colors.blue[600],
                side: BorderSide(color: Colors.blue[600]!),
                shape: RoundedRectangleBorder(
                  borderRadius: BorderRadius.circular(25),
                ),
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildSectionTitle(String title, IconData icon) {
    return Row(
      children: [
        Icon(icon, color: Colors.grey[700]),
        SizedBox(width: 8),
        Text(
          title,
          style: TextStyle(
            fontSize: 18,
            fontWeight: FontWeight.bold,
            color: Colors.grey[800],
          ),
        ),
      ],
    );
  }

  Widget _buildInfoRow(String label, String value, IconData icon, Color color) {
    return Container(
      margin: EdgeInsets.only(bottom: 10),
      padding: EdgeInsets.all(12),
      decoration: BoxDecoration(
        color: color.withOpacity(0.1),
        borderRadius: BorderRadius.circular(10),
        border: Border.all(color: color.withOpacity(0.3)),
      ),
      child: Row(
        children: [
          Icon(icon, color: color, size: 20),
          SizedBox(width: 12),
          Text(
            '$label:',
            style: TextStyle(
              fontWeight: FontWeight.w500,
              color: Colors.grey[700],
            ),
          ),
          SizedBox(width: 8),
          Expanded(
            child: Text(
              value,
              style: TextStyle(
                fontWeight: FontWeight.bold,
                color: color,
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildHobbyChip(String hobby) {
    final colors = {
      'Müzik': Colors.purple,
      'Spor': Colors.orange,
      'Okuma': Colors.brown,
      'Seyahat': Colors.teal,
      'Yemek Pişirme': Colors.red,
    };
    
    final color = colors[hobby] ?? Colors.blue;
    
    return Container(
      padding: EdgeInsets.symmetric(horizontal: 15, vertical: 8),
      decoration: BoxDecoration(
        color: color.withOpacity(0.1),
        borderRadius: BorderRadius.circular(20),
        border: Border.all(color: color.withOpacity(0.3)),
      ),
      child: Row(
        mainAxisSize: MainAxisSize.min,
        children: [
          Icon(
            _getHobbyIcon(hobby),
            color: color,
            size: 18,
          ),
          SizedBox(width: 6),
          Text(
            hobby,
            style: TextStyle(
              color: color,
              fontWeight: FontWeight.w500,
            ),
          ),
        ],
      ),
    );
  }

  IconData _getHobbyIcon(String hobby) {
    switch (hobby) {
      case 'Müzik':
        return Icons.music_note;
      case 'Spor':
        return Icons.sports_soccer;
      case 'Okuma':
        return Icons.book;
      case 'Seyahat':
        return Icons.flight;
      case 'Yemek Pişirme':
        return Icons.restaurant;
      default:
        return Icons.favorite;
    }
  }

  void _showShareDialog(BuildContext context) {
    String shareText = '''
🎯 Anket Sonuçlarım:

👤 Ad Soyad: $name $surname
⚥ Cinsiyet: $gender
🎨 Hobiler: ${hobbies.isEmpty ? 'Belirtilmemiş' : hobbies.join(', ')}
🚬 Sigara: ${smokingStatus ? 'Evet' : 'Hayır'}${smokingStatus && cigarettesPerDay > 0 ? ' (Günlük: $cigarettesPerDay adet)' : ''}
''';

    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: Row(
          children: [
            Icon(Icons.share, color: Colors.blue),
            SizedBox(width: 8),
            Text('Paylaş'),
          ],
        ),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            Container(
              padding: EdgeInsets.all(15),
              decoration: BoxDecoration(
                color: Colors.grey[50],
                borderRadius: BorderRadius.circular(8),
                border: Border.all(color: Colors.grey[300]!),
              ),
              child: Text(
                shareText,
                style: TextStyle(fontSize: 13),
              ),
            ),
            SizedBox(height: 15),
            Text(
              'Bu bilgiler panoya kopyalandı!',
              style: TextStyle(
                color: Colors.green[600],
                fontWeight: FontWeight.w500,
              ),
            ),
          ],
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: Text('Tamam'),
          ),
        ],
      ),
    );
  }

class PersonalitySurveyPage extends StatefulWidget {
  @override
  _PersonalitySurveyPageState createState() => _PersonalitySurveyPageState();
}

class _PersonalitySurveyPageState extends State<PersonalitySurveyPage> {
  // Form kontrolcüleri
  final TextEditingController _nameController = TextEditingController();
  final TextEditingController _surnameController = TextEditingController();
  
  // Dropdown değeri
  String? _selectedGender;
  
  // Checkbox değerleri
  bool _music = false;
  bool _sports = false;
  bool _reading = false;
  bool _travel = false;
  bool _cooking = false;
  
  // Switch değeri
  bool _smokingStatus = false;
  
  // Slider değeri
  double _cigarettesPerDay = 0;
  
  final List<String> _genders = ['Erkek', 'Kadın', 'Diğer', 'Belirtmek İstemiyorum'];

  @override
  void dispose() {
    _nameController.dispose();
    _surnameController.dispose();
    super.dispose();
  }

  void _submitForm() {
    // Form validasyonu
    if (_nameController.text.trim().isEmpty || _surnameController.text.trim().isEmpty) {
      _showErrorDialog('Lütfen ad ve soyad alanlarını doldurunuz.');
      return;
    }
    
    if (_selectedGender == null) {
      _showErrorDialog('Lütfen cinsiyetinizi seçiniz.');
      return;
    }
    
    // Form verilerini topla
    String name = _nameController.text.trim();
    String surname = _surnameController.text.trim();
    
    List<String> hobbies = [];
    if (_music) hobbies.add('Müzik');
    if (_sports) hobbies.add('Spor');
    if (_reading) hobbies.add('Okuma');
    if (_travel) hobbies.add('Seyahat');
    if (_cooking) hobbies.add('Yemek Pişirme');
    
    // Sonuç sayfasına git
    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => SurveyResultPage(
          name: name,
          surname: surname,
          gender: _selectedGender!,
          hobbies: hobbies,
          smokingStatus: _smokingStatus,
          cigarettesPerDay: _smokingStatus ? _cigarettesPerDay.round() : 0,
        ),
      ),
    );
  }
  
  void _showErrorDialog(String message) {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: Text('Uyarı'),
        content: Text(message),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: Text('Tamam'),
          ),
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Kişilik Anketi'),
        backgroundColor: Colors.blue[700],
        foregroundColor: Colors.white,
        elevation: 2,
      ),
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
            colors: [Colors.blue[50]!, Colors.white],
          ),
        ),
        child: SingleChildScrollView(
          padding: EdgeInsets.all(20),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: [
              // Başlık bölümü
              Container(
                padding: EdgeInsets.all(16),
                decoration: BoxDecoration(
                  color: Colors.white,
                  borderRadius: BorderRadius.circular(12),
                  boxShadow: [
                    BoxShadow(
                      color: Colors.grey.withOpacity(0.2),
                      blurRadius: 8,
                      offset: Offset(0, 4),
                    ),
                  ],
                ),
                child: Column(
                  children: [
                    Icon(
                      Icons.person_outline,
                      size: 48,
                      color: Colors.blue[700],
                    ),
                    SizedBox(height: 8),
                    Text(
                      'Bu hafta sizden ödev olarak sizden derste gördüğümüz UI ve Layout elemanlarıyla bir Kişilik anketi sayfası yapmanız bekleniyor.',
                      style: TextStyle(
                        fontSize: 16,
                        fontWeight: FontWeight.w500,
                        color: Colors.grey[700],
                      ),
                      textAlign: TextAlign.center,
                    ),
                    SizedBox(height: 12),
                    Text(
                      'Bu sayfada zorunlu olarak olması gereken elemanlar:',
                      style: TextStyle(
                        fontSize: 14,
                        fontWeight: FontWeight.bold,
                        color: Colors.blue[800],
                      ),
                    ),
                  ],
                ),
              ),
              
              SizedBox(height: 20),
              
              // Form bölümü
              Container(
                padding: EdgeInsets.all(20),
                decoration: BoxDecoration(
                  color: Colors.white,
                  borderRadius: BorderRadius.circular(12),
                  boxShadow: [
                    BoxShadow(
                      color: Colors.grey.withOpacity(0.2),
                      blurRadius: 8,
                      offset: Offset(0, 4),
                    ),
                  ],
                ),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    // Ad ve Soyad TextFields
                    Text(
                      '• Adınız ve Soyadınız (TextField)',
                      style: TextStyle(
                        fontSize: 16,
                        fontWeight: FontWeight.w600,
                        color: Colors.grey[800],
                      ),
                    ),
                    SizedBox(height: 12),
                    
                    Row(
                      children: [
                        Expanded(
                          child: TextField(
                            controller: _nameController,
                            decoration: InputDecoration(
                              labelText: 'Adınız',
                              border: OutlineInputBorder(
                                borderRadius: BorderRadius.circular(8),
                              ),
                              prefixIcon: Icon(Icons.person),
                            ),
                          ),
                        ),
                        SizedBox(width: 12),
                        Expanded(
                          child: TextField(
                            controller: _surnameController,
                            decoration: InputDecoration(
                              labelText: 'Soyadınız',
                              border: OutlineInputBorder(
                                borderRadius: BorderRadius.circular(8),
                              ),
                              prefixIcon: Icon(Icons.person_outline),
                            ),
                          ),
                        ),
                      ],
                    ),
                    
                    SizedBox(height: 24),
                    
                    // Cinsiyet Dropdown
                    Text(
                      '• Cinsiyetinizi seçiniz (DropDown Button)',
                      style: TextStyle(
                        fontSize: 16,
                        fontWeight: FontWeight.w600,
                        color: Colors.grey[800],
                      ),
                    ),
                    SizedBox(height: 12),
                    
                    Container(
                      width: double.infinity,
                      padding: EdgeInsets.symmetric(horizontal: 12),
                      decoration: BoxDecoration(
                        border: Border.all(color: Colors.grey),
                        borderRadius: BorderRadius.circular(8),
                      ),
                      child: DropdownButtonHideUnderline(
                        child: DropdownButton<String>(
                          value: _selectedGender,
                          hint: Text('Cinsiyetinizi seçiniz'),
                          isExpanded: true,
                          items: _genders.map((String gender) {
                            return DropdownMenuItem<String>(
                              value: gender,
                              child: Text(gender),
                            );
                          }).toList(),
                          onChanged: (String? newValue) {
                            setState(() {
                              _selectedGender = newValue;
                            });
                          },
                        ),
                      ),
                    ),
                    
                    SizedBox(height: 24),
                    
                    // Hobiler Checkbox
                    Text(
                      '• Reşit misiniz? (Checkbox veya Checkbox Listile)',
                      style: TextStyle(
                        fontSize: 16,
                        fontWeight: FontWeight.w600,
                        color: Colors.grey[800],
                      ),
                    ),
                    SizedBox(height: 12),
                    
                    Text(
                      'Hobilerinizi seçiniz:',
                      style: TextStyle(
                        fontSize: 14,
                        fontWeight: FontWeight.w500,
                        color: Colors.grey[700],
                      ),
                    ),
                    SizedBox(height: 8),
                    
                    Wrap(
                      children: [
                        _buildCheckboxTile('Müzik', _music, (value) {
                          setState(() {
                            _music = value!;
                          });
                        }),
                        _buildCheckboxTile('Spor', _sports, (value) {
                          setState(() {
                            _sports = value!;
                          });
                        }),
                        _buildCheckboxTile('Okuma', _reading, (value) {
                          setState(() {
                            _reading = value!;
                          });
                        }),
                        _buildCheckboxTile('Seyahat', _travel, (value) {
                          setState(() {
                            _travel = value!;
                          });
                        }),
                        _buildCheckboxTile('Yemek Pişirme', _cooking, (value) {
                          setState(() {
                            _cooking = value!;
                          });
                        }),
                      ],
                    ),
                    
                    SizedBox(height: 24),
                    
                    // Sigara Switch
                    Text(
                      '• Sigara kullanıyor musunuz? (Switch listile)',
                      style: TextStyle(
                        fontSize: 16,
                        fontWeight: FontWeight.w600,
                        color: Colors.grey[800],
                      ),
                    ),
                    SizedBox(height: 12),
                    
                    Container(
                      padding: EdgeInsets.all(12),
                      decoration: BoxDecoration(
                        color: Colors.grey[50],
                        borderRadius: BorderRadius.circular(8),
                        border: Border.all(color: Colors.grey[300]!),
                      ),
                      child: Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          Text(
                            'Sigara kullanıyor musunuz?',
                            style: TextStyle(
                              fontSize: 14,
                              fontWeight: FontWeight.w500,
                            ),
                          ),
                          Switch(
                            value: _smokingStatus,
                            onChanged: (value) {
                              setState(() {
                                _smokingStatus = value;
                                if (!value) _cigarettesPerDay = 0;
                              });
                            },
                          ),
                        ],
                      ),
                    ),
                    
                    // Sigara Slider (eğer sigara kullanıyorsa)
                    if (_smokingStatus) ...[
                      SizedBox(height: 16),
                      Text(
                        '[Eğer evetse ⇒ Altta bir slider çıksın oradan günde kaç tane sigaranın kullanıldığı seçilebilsin]',
                        style: TextStyle(
                          fontSize: 12,
                          fontStyle: FontStyle.italic,
                          color: Colors.blue[600],
                        ),
                      ),
                      SizedBox(height: 8),
                      
                      Container(
                        padding: EdgeInsets.all(12),
                        decoration: BoxDecoration(
                          color: Colors.blue[50],
                          borderRadius: BorderRadius.circular(8),
                          border: Border.all(color: Colors.blue[200]!),
                        ),
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            Text(
                              'Günlük sigara adedi: ${_cigarettesPerDay.round()}',
                              style: TextStyle(
                                fontSize: 14,
                                fontWeight: FontWeight.w500,
                              ),
                            ),
                            Slider(
                              value: _cigarettesPerDay,
                              min: 0,
                              max: 40,
                              divisions: 40,
                              label: '${_cigarettesPerDay.round()} adet',
                              onChanged: (value) {
                                setState(() {
                                  _cigarettesPerDay = value;
                                });
                              },
                            ),
                          ],
                        ),
                      ),
                    ],
                    
                    SizedBox(height: 24),
                    
                    // Alt bilgi
                    Container(
                      padding: EdgeInsets.all(12),
                      decoration: BoxDecoration(
                        color: Colors.amber[50],
                        borderRadius: BorderRadius.circular(8),
                        border: Border.all(color: Colors.amber[200]!),
                      ),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Text(
                            '[Eğer hayırsa ⇒ slider çıkmasın]',
                            style: TextStyle(
                              fontSize: 12,
                              fontStyle: FontStyle.italic,
                              color: Colors.amber[700],
                            ),
                          ),
                          SizedBox(height: 8),
                          Text(
                            '- Bilgilerimi gönder butonu tıklandığında sayfanın en altında derste yaptığımız gibi bir container çıksın ve bilgiler orada gözüksün',
                            style: TextStyle(
                              fontSize: 12,
                              color: Colors.amber[800],
                            ),
                          ),
                        ],
                      ),
                    ),
                    
                    SizedBox(height: 20),
                    
                    // Gönder butonu
                    SizedBox(
                      width: double.infinity,
                      height: 50,
                      child: ElevatedButton(
                        onPressed: _submitForm,
                        style: ElevatedButton.styleFrom(
                          backgroundColor: Colors.blue[700],
                          foregroundColor: Colors.white,
                          shape: RoundedRectangleBorder(
                            borderRadius: BorderRadius.circular(8),
                          ),
                        ),
                        child: Text(
                          'Bilgilerimi Gönder',
                          style: TextStyle(
                            fontSize: 16,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                      ),
                    ),
                  ],
                ),
              ),
              
              SizedBox(height: 20),
              
              // Alt bilgi
              Container(
                padding: EdgeInsets.all(16),
                decoration: BoxDecoration(
                  color: Colors.grey[100],
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Column(
                  children: [
                    Text(
                      'Uygulamanın tasarımını tamamen size bırakıyorum renk ve dizilik kısmını isterseniz row ve column widget\'larından yararlanabilirsiniz.',
                      style: TextStyle(
                        fontSize: 13,
                        color: Colors.grey[600],
                      ),
                      textAlign: TextAlign.center,
                    ),
                    SizedBox(height: 8),
                    Text(
                      'Aşağıda kendim yaptığım örnek bir tasarım var. Bu tasarımdan da örnek alabilirsiniz:',
                      style: TextStyle(
                        fontSize: 13,
                        fontWeight: FontWeight.w500,
                        color: Colors.grey[700],
                      ),
                      textAlign: TextAlign.center,
                    ),
                  ],
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildCheckboxTile(String title, bool value, Function(bool?) onChanged) {
    return Container(
      width: MediaQuery.of(context).size.width * 0.45,
      child: CheckboxListTile(
        title: Text(
          title,
          style: TextStyle(fontSize: 13),
        ),
        value: value,
        onChanged: onChanged,
        controlAffinity: ListTileControlAffinity.leading,
        dense: true,
      ),
    );
  }
}
