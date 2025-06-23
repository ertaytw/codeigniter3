# 🚀 CodeIgniter 3 Kurulum ve Konfigürasyon Rehberi

Bu rehber, CodeIgniter 3 projenizi sıfırdan kurmak ve yapılandırmak için gereken tüm adımları içerir.

## 📋 İçindekiler

- [Ön Gereksinimler](#-ön-gereksinimler)
- [Kurulum Adımları](#-kurulum-adımları)
- [Konfigürasyon](#-konfigürasyon)
- [Klasör Yapısı](#-klasör-yapısı)
- [Test Etme](#-test-etme)
- [Hata Giderme](#-hata-giderme)
- [Güvenlik](#-güvenlik)

## 🔧 Ön Gereksinimler

- PHP 7.2 veya üzeri
- MySQL/MariaDB veritabanı
- Apache/Nginx web sunucusu
- mod_rewrite aktif

## 📦 Kurulum Adımları

### 1. CodeIgniter 3 İndirme

```bash
# Git ile klonlama
git clone https://github.com/bcit-ci/CodeIgniter.git
cd CodeIgniter

# Veya direkt indirme
wget https://github.com/bcit-ci/CodeIgniter/archive/3.1.13.zip
```

### 2. Klasör İzinleri

```bash
chmod 755 application/logs
chmod 755 application/cache
chmod 755 assets/uploads
```

## ⚙️ Konfigürasyon

### 1. Veritabanı Ayarları

**Dosya:** `application/config/database.php`

```php
$db['default'] = array(
    'dsn'      => '',
    'hostname' => 'localhost',
    'username' => 'your_username',
    'password' => 'your_password',
    'database' => 'your_database',
    'dbdriver' => 'mysqli',
    'dbprefix' => '',
    'pconnect' => FALSE,
    'db_debug' => (ENVIRONMENT !== 'production'),
    'cache_on' => FALSE,
    'cachedir' => '',
    'char_set' => 'utf8',
    'dbcollat' => 'utf8_general_ci',
    'swap_pre' => '',
    'encrypt'  => FALSE,
    'compress' => FALSE,
    'stricton' => FALSE,
    'failover' => array(),
    'save_queries' => TRUE
);
```

### 2. Base URL Konfigürasyonu

**Dosya:** `application/config/config.php`

```php
// Geliştirme ortamı
$config['base_url'] = 'http://localhost/your_project/';

// Canlı ortam
$config['base_url'] = 'https://yourdomain.com/';

// Dinamik ayar (önerilen)
$config['base_url'] = ((isset($_SERVER['HTTPS']) && $_SERVER['HTTPS'] == "on") ? "https" : "http");
$config['base_url'] .= "://".$_SERVER['HTTP_HOST'];
$config['base_url'] .= str_replace(basename($_SERVER['SCRIPT_NAME']),"",$_SERVER['SCRIPT_NAME']);
```

### 3. Güvenlik Anahtarı

**Dosya:** `application/config/config.php`

```php
// 32 karakterlik rastgele anahtar oluşturun
$config['encryption_key'] = 'your-32-character-secret-key-here';

// Örnek:
$config['encryption_key'] = 'ilanpazar2024secretkey123456789abc';
```

### 4. URL'den index.php Kaldırma

**a) Config Ayarı:**
```php
// application/config/config.php
$config['index_page'] = '';
```

**b) .htaccess Dosyası:**

Proje ana dizininde `.htaccess` dosyası oluşturun:

```apache
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php/$1 [QSA,L]

# index.php'yi URL'den kaldır
RewriteCond %{THE_REQUEST} /index\.php/([^\s\?]*) [NC]
RewriteRule ^ /%1 [R=302,L]
```

### 5. Autoload Ayarları

**Dosya:** `application/config/autoload.php`

```php
$autoload['packages'] = array();
$autoload['libraries'] = array('database', 'session', 'form_validation');
$autoload['drivers'] = array();
$autoload['helper'] = array('url', 'form', 'html', 'security');
$autoload['config'] = array();
$autoload['language'] = array();
$autoload['model'] = array();
```

### 6. Routing Ayarları

**Dosya:** `application/config/routes.php`

```php
$route['default_controller'] = 'home';
$route['404_override'] = '';
$route['translate_uri_dashes'] = FALSE;

// Özel route'lar
$route['yang'] = 'listings/yang';
$route['items'] = 'listings/items';
$route['accounts'] = 'listings/accounts';
$route['profile'] = 'user/profile';
$route['login'] = 'auth/login';
$route['register'] = 'auth/register';
```

### 7. Session Konfigürasyonu

**Dosya:** `application/config/config.php`

```php
$config['sess_driver'] = 'files';
$config['sess_cookie_name'] = 'ci_session';
$config['sess_expiration'] = 7200;
$config['sess_save_path'] = NULL;
$config['sess_match_ip'] = FALSE;
$config['sess_time_to_update'] = 300;
$config['sess_regenerate_destroy'] = FALSE;
```

### 8. Saat Dilimi

**Dosya:** `application/config/config.php` (en üst kısım)

```php
date_default_timezone_set('Europe/Istanbul');
```

### 9. Environment Ayarı

**Dosya:** `index.php`

```php
// Geliştirme ortamı için
define('ENVIRONMENT', 'development');

// Canlı ortam için
// define('ENVIRONMENT', 'production');
```

## 📁 Klasör Yapısı

```
your_project/
├── 📁 application/
│   ├── 📁 config/
│   │   ├── autoload.php
│   │   ├── config.php
│   │   ├── database.php
│   │   └── routes.php
│   ├── 📁 controllers/
│   │   └── Home.php
│   ├── 📁 models/
│   ├── 📁 views/
│   │   ├── 📁 templates/
│   │   │   ├── header.php
│   │   │   └── footer.php
│   │   └── 📁 home/
│   │       └── index.php
│   └── 📁 logs/
├── 📁 assets/
│   ├── 📁 css/
│   ├── 📁 js/
│   ├── 📁 images/
│   └── 📁 uploads/
├── 📁 system/
├── .htaccess
├── index.php
└── README.md
```

## 📝 Temel Controller Örneği

**Dosya:** `application/controllers/Home.php`

```php
<?php
defined('BASEPATH') OR exit('No direct script access allowed');

class Home extends CI_Controller {

    public function __construct() {
        parent::__construct();
        // Model yükleme örneği
        // $this->load->model('user_model');
    }

    public function index() {
        $data['title'] = 'İlanPazar - Ana Sayfa';
        $data['description'] = 'Oyun içi ürünler için güvenilir platform';
        
        $this->load->view('templates/header', $data);
        $this->load->view('home/index', $data);
        $this->load->view('templates/footer');
    }
}
```

## 🧪 Test Etme

### 1. Temel Test

Tarayıcınızda projenizi açın:
```
http://localhost/your_project/
```

### 2. Database Test

Basit bir test controller'ı oluşturun:

```php
public function db_test() {
    $query = $this->db->query("SELECT VERSION() as version");
    $result = $query->row();
    echo "Database bağlantısı başarılı! MySQL Version: " . $result->version;
}
```

### 3. Session Test

```php
public function session_test() {
    $this->session->set_userdata('test', 'Session çalışıyor!');
    echo $this->session->userdata('test');
}
```

## 🐛 Hata Giderme

### Yaygın Hatalar ve Çözümleri

| Hata | Çözüm |
|------|-------|
| `The configuration file does not exist` | `config.php` dosyasının varlığını kontrol edin |
| `Unable to connect to your database server` | Database ayarlarınızı kontrol edin |
| `404 Page Not Found` | `.htaccess` ve `base_url` ayarlarını kontrol edin |
| `The page you requested was not found` | `routes.php` dosyasını kontrol edin |
| `Session: Configured save path is not writable` | `application/logs` klasörünün izinlerini kontrol edin |

### Debug Modu

Geliştirme sırasında hataları görmek için:

```php
// application/config/config.php
$config['log_threshold'] = 4;

// index.php
define('ENVIRONMENT', 'development');
```

## 🔒 Güvenlik

### Checklist

- [ ] `encryption_key` değiştirildi
- [ ] Database şifreleri güçlü
- [ ] Production'da error reporting kapatıldı
- [ ] `uploads` klasörüne güvenlik eklenildi
- [ ] `system` klasörü web erişiminden çıkarıldı
- [ ] XSS ve CSRF koruması aktif

### Uploads Klasörü Güvenliği

`assets/uploads/.htaccess`:
```apache
<Files "*">
    Order Deny,Allow
    Deny from all
</Files>

<FilesMatch "\.(jpg|jpeg|png|gif|pdf)$">
    Order Allow,Deny
    Allow from all
</FilesMatch>
```

## 📚 Yararlı Linkler

- [CodeIgniter 3 Dokümantasyonu](https://codeigniter.com/userguide3/)
- [CodeIgniter 3 GitHub](https://github.com/bcit-ci/CodeIgniter)
- [Türkçe CodeIgniter Topluluğu](https://codeigniterturkiye.net/)

## 🤝 Katkıda Bulunma

1. Bu repo'yu fork edin
2. Feature branch oluşturun (`git checkout -b feature/yeni-ozellik`)
3. Değişikliklerinizi commit edin (`git commit -am 'Yeni özellik eklendi'`)
4. Branch'inizi push edin (`git push origin feature/yeni-ozellik`)
5. Pull Request oluşturun

## 📄 Lisans

Bu proje MIT lisansı altında lisanslanmıştır.

---

**⚡ Hızlı Başlangıç:** Yukarıdaki adımları takip ettikten sonra CodeIgniter projeniz çalışmaya hazır olacaktır!

**📞 Destek:** Herhangi bir sorunla karşılaştığınızda issue açabilirsiniz.
