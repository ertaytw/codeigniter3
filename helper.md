# Helper Kullanım Kılavuzu

## Kurulum

### 1. Autoload Ayarı

```php
// application/config/autoload.php
$autoload['helper'] = array('url', 'seo');
$autoload['model'] = array('comodel');
```

### 2. Tek Dosya

Sadece `application/helpers/seo_helper.php` dosyası yeterli!

## Kullanım

### SEO Fonksiyonları

```php
// View'da
<?= seo_tags('anasayfa') ?>
<?= seo_tags('kategori') ?>
<?= seo_tags('ilan') ?>

// Sadece title/description
<?= seo_title('anasayfa') ?>
<?= seo_description('kategori') ?>
```

### Site Bilgileri

```php
// Temel bilgiler
<?= site_title() ?>           // Metin2 Marketplace
<?= site_description() ?>     // Site açıklaması
<?= site_logo() ?>            // Logo URL
<?= site_favicon() ?>         // Favicon URL

// Durum kontrolleri
<?php if(site_aktif_mi()): ?>
    Site aktif
<?php endif; ?>

<?php if(bakim_modu_aktif_mi()): ?>
    <?= bakim_mesaji() ?>
<?php endif; ?>
```

### Dil ve Zaman

```php
// Dil
<?= varsayilan_dil() ?>       // tr
<?php foreach(desteklenen_diller() as $dil): ?>
    <a href="/lang/<?= $dil ?>"><?= $dil ?></a>
<?php endforeach; ?>

// Tarih
<?= format_tarih() ?>                    // 23.06.2025 14:30
<?= format_tarih(time()) ?>              // Şu anki zaman
<?= format_tarih(strtotime('2025-01-01')) ?>  // Belirli tarih
```

### Para ve Formatlaştırma

```php
// Para formatla
<?= format_para(1500) ?>        // 1.500,00 ₺
<?= format_para(1500, 'USD') ?>  // 1.500,00 $
<?= format_para(2500.50) ?>     // 2.500,50 ₺

// Para birimi bilgileri
<?= para_birimi() ?>            // TRY
<?php foreach(desteklenen_para_birimleri() as $para): ?>
    <option value="<?= $para ?>"><?= $para ?></option>
<?php endforeach; ?>
```

### Sayfalama

```php
// İlan sayıları
<?= sayfa_basina_ilan() ?>       // 20
<?= anasayfa_ilan_sayisi() ?>    // 12

// Controller'da
$limit = sayfa_basina_ilan();
$ilanlar = $this->comodel->get_limit('ilanlar', $limit);
```

### Ödeme Limitleri

```php
// Bakiye limitleri
Min: <?= format_para(minimum_bakiye()) ?>         // 10,00 ₺
Max: <?= format_para(maksimum_bakiye()) ?>        // 10.000,00 ₺

// Çekim limitleri
Min: <?= format_para(minimum_cekim()) ?>          // 50,00 ₺
Max: <?= format_para(maksimum_cekim()) ?>         // 5.000,00 ₺

// Komisyon
Oran: %<?= cekim_komisyon() ?>                    // 2.50
```

### Ödeme Yöntemleri

```php
// Ödeme yöntemleri
<?php foreach(odeme_yontemleri() as $yontem): ?>
    <option value="<?= $yontem ?>"><?= $yontem ?></option>
<?php endforeach; ?>

// Çekim yöntemleri
<?php foreach(cekim_yontemleri() as $yontem): ?>
    <option value="<?= $yontem ?>"><?= $yontem ?></option>
<?php endforeach; ?>
```

### İş Mantığı Fonksiyonları

```php
// Komisyon hesapla
<?php
$tutar = 1000;
$komisyon = hesapla_cekim_komisyonu($tutar);
echo "Komisyon: " . format_para($komisyon);
?>

// Limit kontrolleri
<?php if(bakiye_yukleme_gecerli_mi($tutar)): ?>
    Geçerli tutar
<?php else: ?>
    Hatalı tutar
<?php endif; ?>

// Yöntem kontrolleri
<?php if(odeme_yontemi_aktif_mi('papara')): ?>
    <option value="papara">Papara</option>
<?php endif; ?>
```

## Örnek Controller

```php
class Anasayfa extends MY_Controller
{
    public function __construct()
    {
        parent::__construct();
        bakimi_kontrol_et(); // Bakım modu kontrolü
    }

    public function index()
    {
        $limit = anasayfa_ilan_sayisi();
        $data['ilanlar'] = $this->comodel->get_limit('ilanlar', $limit);

        $this->load->view('templates/header');
        $this->load->view('anasayfa', $data);
        $this->load->view('templates/footer');
    }
}
```

## Örnek View

```php
<!DOCTYPE html>
<html lang="<?= varsayilan_dil() ?>">
<head>
    <?= seo_tags('anasayfa') ?>
    <link rel="icon" href="<?= site_favicon() ?>">
</head>
<body>
    <header>
        <img src="<?= site_logo() ?>" alt="<?= site_title() ?>">
        <h1><?= site_title() ?></h1>
    </header>

    <main>
        <?php foreach($ilanlar as $ilan): ?>
            <div class="ilan">
                <h3><?= $ilan->baslik ?></h3>
                <span><?= format_para($ilan->fiyat) ?></span>
                <small><?= format_tarih(strtotime($ilan->olusturma_tarihi)) ?></small>
            </div>
        <?php endforeach; ?>
    </main>
</body>
</html>
```

## Örnek Form

```php
<form method="post">
    <input type="number" name="tutar"
           min="<?= minimum_bakiye() ?>"
           max="<?= maksimum_bakiye() ?>" required>

    <select name="odeme_yontemi">
        <?php foreach(odeme_yontemleri() as $yontem): ?>
            <option value="<?= $yontem ?>"><?= $yontem ?></option>
        <?php endforeach; ?>
    </select>

    <button type="submit">Yükle</button>
</form>
```

Bu kadar basit! Tek helper dosyası ile her şey hazır! 🚀
