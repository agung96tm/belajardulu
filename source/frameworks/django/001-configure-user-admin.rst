*****
Mengatur User Admin
*****

Bagi teman-teman yang ingin mengatur/meng-kustom user admin bawaan Django, kalian bisa mengikuti contoh dibawah ini.


.. code-block:: python

    # point 1
    # users/admin.py
    from django.contrib.auth.models import User
    from django.contrib.auth.admin import UserAdmin as DjangoUserAdmin
    from django.contrib import admin
    from users.models import SocialMedia

    # point 2
    class SocialMediaInline(admin.TabularInline):
        model = SocialMedia
        fields = ['uid', 'social_name', 'link']

    # point 3
    class UserAdmin(DjangoUserAdmin):
        inlines = [SocialMediaInline]

    # point 4
    admin.site.unregister(User)
    admin.site.register(User, UserAdmin)


Mari kita analisa per-point:

Point 1: Import
**********************

* Bagian komentar dijelaskan bahwa kodingan ini ditaruh dalam user app dan didefinisikan kedalam admin.py
* Meng-import User bawaan Django, kita akan gunakan di point nomor 4
* Meng-import UserAdmin bawaan Django, kita memberikan inisial "DjangoUserAdmin" agar tidak bentrok dengan class yang akan didefinisikan di point nomor 3
* Kita juga mengimport SocialMedia yang akan digunakan sebagai Inline class


Point 2: Inline Class
**********************

* Membuat `Inline class <https://docs.djangoproject.com/en/4.0/ref/contrib/admin/#inlinemodeladmin-objects>`_ yang akan digunakan oleh UserAdmin di point nomor 3.
* Kita sertakan model dan fields, isi fields sesuai dengan field-field yang ada model.


Point 3: UserAdmin (Custom)
**********************

* Di class ini kita meng-extends DjangoUserAdmin (UserAdmin bawaan Django)
* Kita juga me-"register" SocialMediaInline yg dibuat pada point nomor 2


Point 4: Unregister dan Register
**********************

* kita meminta Django untuk meng-unregister admin bawaan Django
* Dan line terakhir ini berfungsi "menggantikan" class admin bawaan Django dengan class yang sudah didefinisikan dalam point nomor 3


Example
**********************
- Github: https://github.com/contohdulu/example-django-useradmin
