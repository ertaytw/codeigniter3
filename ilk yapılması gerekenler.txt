==========================================
   CODEIGNITER 3 TEMEL KURULUM
==========================================

1. VERİTABANI BAĞLANTISI
==========================================

Dosya: application/config/database.php

$db['default'] = array(
    'hostname' => 'localhost',
    'username' => 'veritabani_kullanici_adi',
    'password' => 'veritabani_sifresi', 
    'database' => 'veritabani_adi',
    'dbdriver' => 'mysqli',
    'char_set' => 'utf8',
    'dbcollat' => 'utf8_general_ci',
    'db_debug' => TRUE
);

==========================================

2. BASE URL AYARI
==========================================
Dosya: application/config/config.php

$config['base_url'] = 'http://localhost/proje_adi/';

ÖRNEKLER:
- Localhost: http://localhost/ilanpazar/
- Canlı site: https://www.siteniz.com/
- Alt dizin: https://www.siteniz.com/proje/

==========================================
3. GÜVENLİK ANAHTARI
==========================================
Dosya: application/config/config.php

$config['encryption_key'] = 'rastgele-32-karakter-anahtar-buraya';

ÖRNEK:
$config['encryption_key'] = 'ilanpazar2024secretkey123456789';

NOT: Bu anahtar session ve şifreleme için kritik öneme sahiptir!

==========================================
4. URL'DEN index.php KALDIRMA
==========================================

4a) Config dosyasını düzenleyin:
Dosya: application/config/config.php

$config['index_page'] = '';

4b) .htaccess dosyası oluşturun:
Dosya: .htaccess (ana dizinde, index.php ile aynı seviyede)

RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php/$1 [QSA,L]

==========================================
5. OTOMATIK YÜKLEME AYARLARI
==========================================
Dosya: application/config/autoload.php

$autoload['helper'] = array('url', 'form', 'html');
$autoload['libraries'] = array('database', 'session');

Bu ayar sayesinde her controller'da tekrar tekrar yükleme yapmanıza gerek kalmaz.

==========================================
6. VARSAYILAN CONTROLLER
==========================================
Dosya: application/config/routes.php

$route['default_controller'] = 'home';
$route['404_override'] = '';
$route['translate_uri_dashes'] = FALSE;

Bu ayar sitenizin ana sayfasını belirler.

==========================================
7. SESSION AYARLARI
==========================================
Dosya: application/config/config.php

$config['sess_driver'] = 'files';
$config['sess_cookie_name'] = 'ci_session';
$config['sess_expiration'] = 7200;
$config['sess_save_path'] = NULL;
$config['sess_match_ip'] = FALSE;
$config['sess_time_to_update'] = 300;

==========================================
8. SAAT DİLİMİ AYARI
==========================================
Dosya: application/config/config.php (en üst kısma ekleyin)

date_default_timezone_set('Europe/Istanbul');

==========================================
9. TEMEL CONTROLLER OLUŞTURMA
==========================================
Dosya: application/controllers/Home.php

<?php
defined('BASEPATH') OR exit('No direct script access allowed');

class Home extends CI_Controller {

    public function __construct() {
        parent::__construct();
    }

    public function index() {
        $data['title'] = 'İlanPazar - Ana Sayfa';
        
        $this->load->view('templates/header', $data);
        $this->load->view('home/index', $data);
        $this->load->view('templates/footer');
    }
}

==========================================
10. KLASÖR YAPISI
==========================================

proje_adi/
├── application/
│   ├── config/
│   │   ├── config.php
│   │   ├── database.php
│   │   ├── routes.php
│   │   └── autoload.php
│   ├── controllers/
│   │   └── Home.php
│   ├── models/
│   ├── views/
│   │   ├── templates/
│   │   │   ├── header.php
│   │   │   └── footer.php
│   │   └── home/
│   │       └── index.php
│   └── logs/
├── assets/
│   ├── css/
│   ├── js/
│   ├── images/
│   └── uploads/
├── system/
├── .htaccess
└── index.php

==========================================
11. TEST ETME
==========================================

Kurulumunuzu test etmek için:

1. Tarayıcıda sitenizi açın: http://localhost/proje_adi/
2. Hata yoksa CodeIgniter welcome mesajını görmelisiniz
3. Database bağlantısını test edin
4. Session'ları test edin

==========================================
12. HATA GİDERME
==========================================

Sık Karşılaşılan Hatalar:

HATA: "The configuration file does not exist"
ÇÖZÜM: config.php dosyasının varlığını kontrol edin

HATA: "Unable to connect to your database server"
ÇÖZÜM: Database ayarlarınızı kontrol edin

HATA: "404 Page Not Found"
ÇÖZÜM: .htaccess dosyasını kontrol edin, base_url'i kontrol edin

HATA: "The page you requested was not found"
ÇÖZÜM: routes.php dosyasını kontrol edin

==========================================
13. GÜVENLİK NOTLARI
==========================================

✓ encryption_key'i mutlaka değiştirin
✓ Database şifrelerini güçlü yapın
✓ Canlı ortamda error_reporting'i kapatın
✓ Uploads klasörüne .htaccess ekleyin
✓ System klasörünü web erişiminden çıkarın

==========================================
14. ÖNEMLİ NOTLAR
==========================================

• Bu ayarları yapmadan CodeIgniter düzgün çalışmaz
• Her ayarı kendi proje ihtiyaçlarınıza göre düzenleyin  
• Canlı ortama geçerken base_url'i değiştirmeyi unutmayın
• Backup almayı ihmal etmeyin
• Error log'larını düzenli kontrol edin


==========================================