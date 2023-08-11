
# نصب و ستاپ جنکیز

# نصب جنکینز در اوبونتو

## مقدمه
 در این راهنما، نحوه نصب جنکینز در سیستم عامل اوبونتو را بررسی خواهیم کرد.

## مراحل نصب

### 1. بروزرسانی سیستم
ابتدا باید مخازن سیستم را به روزرسانی و آپگرید کنیم:

```bash
sudo apt update
sudo apt upgrade
```
### 2. نصب جاوا
جنکینز نیازمند نصب جاوا در سیستم است. برای نصب جاوا، دستورات زیر را در ترمینال وارد میکنیم:

```bash
sudo apt install openjdk-17-jre
```

### 3. دریافت امضای کلید مخزن
برای نصب جنکینز، ابتدا باید امضای کلید مخزن را دریافت کنیم. برای این کار، دستور زیر را در ترمینال وارد کنید:

```bash
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
```

### 4. افزودن مخزن جنکینز
حالا باید مخزن جنکینز را به لیست منابع نصب اوبونتو اضافه کنیم. برای این کار، دستور زیر را در ترمینال وارد کنید:

```bash
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
```

### 5. بروزرسانی مجدد مخازن
حالا لازم است لیست منابع نصب هریزه را بروزرسانی کنیم تا تغییراتی که در مرحله قبل اعمال کردیم به روز شود. برای این کار، از دستور زیر استفاده کنید:

```bash
sudo apt update
```

### 6. نصب جنکینز
و در آخر، با استفاده از دستور زیر، می‌توانید جنکینز را در سیستم عامل اوبونتو نصب کنید:

 ```bash
sudo apt install jenkins
```

## راه اندازی جنکینز
حالا که جنکینز نصب شده است، باید سرویس آن را راه‌اندازی کنیم.

### 1. شروع سرویس
با استفاده از دستور زیر، می‌توانید سرویس جنکینز را راه‌اندازی کنید:

```bash
sudo systemctl start jenkins
```

### 2. بررسی وضعیت سرویس
برای بررسی وضعیت سرویس جنکینز، از دستور زیر استفاده کنید:

```bash
sudo systemctl status jenkins
```

در صورتی که سرویس به درستی راه‌اندازی شده باشد، وضعیت آن باید به عنوان "active" نمایش داده شود.

### 3. اتمام فرآیند نصب
پس از راه‌اندازی سرویس، می‌توان به مرورگر وارد شده و آدرس `http://localhost:8080` را وارد کنید. این کار به شما اجازه می‌دهد که مراحل تکمیلی نصب جنکینز را انجام دهید.


### ستاپ و تنظیم کردن دستر سی
اگر از Nginx برای وب سرور استفاده میشود. و قرار است پروژه در پوشه www باشد، باید دسترسی جنکینز باید دسترسی به پوشه داشته باشد.

مثلاً از دستور زیر میتوان استفاده کرد:
```bash
 sudo chown -R jenkins:www-data /var/www/
```

همچنین بهتر است دسترسی به پوشه های لاراول هم تنظیم شود مثلا اینگونه:
```bash
sudo chmod -R 755 /var/www/
```


### ایجاد Pipline

در جنکینز از سودو پایپلاین زیر میتوان استفاده کرد:

```angular2html
pipeline {
    agent any

    stages {
        stage('SCM Dev Pull') {
            steps {
                // here fetch main branch from git
                git branch: 'main', url: 'https://github.com/mmasomi73/laravel-jenkins.git'
            }
        }
        stage('Test'){
            steps{
                sh "composer install"
                sh "npm install"
            }
        }
        stage('Build'){
            steps{
                echo 'npm run build'
            }
        }
        stage('Production'){
            steps{
                dir('/var/www/laravel-jenkins'){
                    sh 'php artisan down'
                    sh "git reset --hard HEAD"
                    sh "git pull origin main"
                    sh "composer install"
                    sh "npm install"
                    sh "npm run build"
                    sh "php artisan up"
                }

            }
        }
    }
}

```
