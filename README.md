# Wedding Invitation Website (English + Khmer)

A responsive wedding invitation website built with PHP, MySQL, HTML5, CSS3, Bootstrap 5 and jQuery.

## Features
- English / Khmer interface switch
- Couple profiles and love story timeline
- Wedding events, venue and Google Maps links
- Photo gallery with Bootstrap modal lightbox
- Countdown to the wedding date
- RSVP form saved to MySQL using PDO prepared statements
- Admin login, RSVP dashboard and CSV export
- CSRF protection, output escaping and server-side validation

## Docker deployment with an existing MySQL server

This project uses a PHP/Apache container only. It does not create a second MySQL server.

1. Extract the project ZIP into your server.
2. Create a database and application user in your existing MySQL server:
   ```sql
   CREATE DATABASE IF NOT EXISTS wedding_invitation CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
   CREATE USER IF NOT EXISTS 'wedding_app'@'%' IDENTIFIED BY 'YOUR_STRONG_PASSWORD';
   GRANT SELECT, INSERT, UPDATE, DELETE ON wedding_invitation.* TO 'wedding_app'@'%';
   FLUSH PRIVILEGES;
   ```
3. Import `database.sql` into MySQL:
   ```bash
   mysql -u root -p < database.sql
   ```
4. Configure `.env` from `.env.example`. For MySQL installed directly on the Ubuntu host, use `DB_HOST=host.docker.internal`, set `DB_NAME=wedding_invitation`, `DB_USER=wedding_app`, and set your password in `DB_PASS`.
5. Start the website:
   ```bash
   docker compose up -d --build
   docker compose ps
   docker compose logs -f wedding-web
   ```
6. Open `http://YOUR_SERVER_IP:8081/`. Port 8081 avoids colliding with the existing service on port 8080.

If MySQL runs in another Docker container, connect both containers to the same user-defined Docker network and set `DB_HOST` to the MySQL container's network alias. Do not expose MySQL port 3306 publicly; allow access only from the application host/container network.

## Local XAMPP setup
1. Copy the project folder into `C:\\xampp\\htdocs\\`.
2. Start Apache and MySQL.
3. Import `database.sql` with phpMyAdmin.
4. Configure the database connection in `config/db.php`.
5. Open `http://localhost/wedding-invitation-project/`.

## Admin
- URL: `/admin/login.php`
- Demo username: `admin`
- Demo password: `password`

**Important:** Change the demo admin password before public deployment. Use HTTPS, keep `.env` private, use a least-privilege MySQL account, and back up the database regularly.

## Customization
Edit the sample names, wedding date, couple details, story, events and gallery records in `database.sql` or with phpMyAdmin. Replace placeholder images in `assets/images/` with your own licensed photos. Database-entered content is displayed as entered; add Khmer translations to those values as needed.

This starter does not include an admin content editor or photo-upload screen; content updates are made through phpMyAdmin or by extending the admin area.
