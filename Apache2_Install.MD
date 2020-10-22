어떤 Tweak 을 설치하기 전 apt update 는 습관적으로 하는것이 좋다.

```bash
sudo apt update
sudo apt upgrade
```

- **Apache2 설치하기**

    ```bash
    apt install apache2
    ```

- **PHP 설치하기**

    ```bash
    sudo apt-get install php libapache2-mod-php php-xml php-gd php-mysql
    sudo apt-get install php7.2-mysql php7.2-curl php7.2-xml php7.2-zip php7.2-gd php7.2-mbstring

    ** 2020년 6월 기준 php7.2 안되고 7.4 깔아야하네
    ```
