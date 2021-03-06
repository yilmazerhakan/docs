# İstekler (Requests) ve Girdi (Input)

- [Basit Girdi](#basic-input)
- [Çerezler (Cookies)](#cookies)
- [Önceki Girdi](#old-input)
- [Dosyalar](#files)
- [İstek Bilgileri](#request-information)

<a name="basic-input"></a>
## Basit Girdi

Tüm kullanıcı girdisine birkaç basit yöntemle erişebilirsiniz. İstek için kullanılmış olan HTTP eylemi için endişe etmenize gerek yoktur, bütün eylemler için girdi bilgisine erişim aynıdır.

**Bir Girdi Değerinin Öğrenilmesi**

	$ismi = Input::get('ismi');

**Bir Girdi Değerinin (Eksik Olması Durumunda Varsayılacak Olan Bir "Ön Değer" Belirtilerek) Öğrenilmesi**

	$ismi = Input::get('ismi', 'Saliha');

**Bir Girdi Değerinin Mevcut Olduğunun Test Edilmesi**

	if (Input::has('ismi'))
	{
		//
	}

**İstekteki Tüm Girdi Değerlerinin Birden Öğrenilmesi**

	$girdi = Input::all();

**İstek Girdisinin Sadece Bazı Değerlerinin Öğrenilmesi**

	$girdi = Input::only('kullaniciadi', 'sifre'); 	//sadece belirtilenler

	$girdi = Input::except('kredi_karti');	//belirtilenler hariç

Bazı JavaScript kütüphaneleri, örneğin Backbone, girdi bilgisini uygulamaya JSON olarak gönderir. Bu girdi verisine de yine normal şekilde `Input::get` ile erişebilirsiniz.

<a name="cookies"></a>
## Çerezler (Cookies)

Laravel çerçevesi tarafından oluşturulan tüm çerezler, bir kimlik doğrulama kodu ile şifrelenir ve imzalanır. Kullanıcı tarafından değiştirilmesi halinde geçersiz kabul edilecektir.

**Bir Çerez Değerinin Öğrenilmesi**

	$deger = Cookie::get('ismi');

**Yanıta(Response) Yeni Bir Çerez İliştirilmesi**

	$yanıt= Response::make('Merhaba Dünya');

	$yanıt->withCookie(Cookie::make('ismi', 'degeri', $dakika));

**Süresiz Bir Çerez Oluşturulması**

	$cerez = Cookie::forever('ismi', 'degeri');

<a name="old-input"></a>
## Önceki Girdi

Bazı durumlarda bir isteğin girdisini bir sonraki isteğe kadar tutmanız gerekebilir. Örneğin, doğrulama hataları için kontrol ettikten sonra bir formu yeniden bu önceki girdi bilgisi ile doldurmak gerekebilir.

**Girdinin Oturuma(Session) Geçici Olarak Yansıtılması (flash)**

	Input::flash();

**Girdinin Sadece Bazı Değerlerinin Oturuma Geçici Olarak Yansıtılması**

	Input::flashOnly('kullaniciadi', 'email');	//sadece belirtilenler

	Input::flashExcept('sifre');	//belirtilenler hariç

Girdinin geçici olarak oturuma yansıtılmasını, sık şekilde bir önceki sayfaya tekrar-yönlendirme (redirect) ile birlikte yapacağınız için, bu yansıtmayı (redirect)'e zincir ek yapabilirsiniz.

	return Redirect::to('form')->withInput();	//tüm girdi değerleri ile beraber

	return Redirect::to('form')->withInput(Input::except('sifre'));	//belirtilenler hariç

> **Not:** Diğer verilerin istekler arasında geçici yansıtmasını (flash), Oturum [Session](/docs/session) sınıfını kullanarak  yapabilirsiniz.

**Önceki Girdi Verisinin Elde Edilmesi**

	Input::old('kullaniciadi');

<a name="files"></a>
## Dosyalar

**Yollanan Bir Dosyanın Öğrenilmesi**

	$dosya = Input::file('foto');

**Bir Dosya Yollanmış Olup Olmadığının Belirlenmesi**

	if (Input::hasFile('foto'))
	{
		//
	}

Dosya `file` metodu tarafından döndürülen nesne (object), PHP `SplFileInfo` sınıfının bir uzantısı olan `Symfony\Component\HttpFoundation\File\UploadedFile` sınıfının bir "üyesidir", ve bu sayede dosya ile etkileşim için çeşitli yöntemler sağlar.

**Yüklenmiş Olan Bir Dosyanın Taşınması**

	Input::file('foto')->move($hedefDizinPatikasi);

	Input::file('foto')->move($hedefDizinPatikasi, $dosyaAdi);

**Yüklenmiş Olan Bir Dosyanın Dosya Yolunun Öğrenilmesi**

	$patika = Input::file('foto')->getRealPath();

**Yüklenmiş Olan Bir Dosyanın Orijinal Adının Öğrenilmesi

	$name = Input::file('foto')->getClientOriginalName();

**Yüklenmiş Olan Bir Dosyanın Boyutunun Öğrenilmesi**

	$buyukluk = Input::file('foto')->getSize();

**Yüklenmiş Olan Bir Dosyanın MIME Türünün Öğrenilmesi**

	$mime = Input::file('foto')->getMimeType();

<a name="request-information"></a>
## İstek Bilgileri

İstek `Request` sınıfı, uygulamanıza gelecek olan HTTP isteğini incelemeniz için birçok yöntem sunar ve `Symfony\Component\HttpFoundation\Request` sınıfının bir uzantısıdır. Bunlardan bazıları şöyledir.

**İstek URI'nın Öğrenilmesi**

	$uri = Request::path();

**İstek Patikasının Bir Şablona Uygunluğunun Test Edilmesi**

	if (Request::is('admin/*'))
	{
		//
	}

**İstek URL'nin Öğrenilmesi**

	$url = Request::url();

**İstek URI'nın Herhangi Bir Bölümünün Öğrenilmesi**

	$segment = Request::segment(1);

**Bir İstek Başlığı(Header) Değerinin Öğrenilmesi**

	$deger = Request::header('Content-Type');

**Sunucu bilgileri için $_SERVER Değerlerinin Öğrenilmesi**

	$deger = Request::server('PATH_INFO');

**İsteğin AJAX Kullandığının Test Edilmesi**

	if (Request::ajax())
	{
		//
	}

**İsteğin HTTPS Üzerinden Olduğunun Test Edilmesi**

	if (Request::secure())
	{
		//
	}
