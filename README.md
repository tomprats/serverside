# Setup

##### Pull for updates to this file

Clone the repo into the source folder

    git clone https://github.com/tomprats/example.git ~rails/source 

Create and add ENV variables

    cp ~rails/source/config/local_variables.example.rb ~rails/source/config/local_variables.rb
    vim ~rails/source/config/local_variables.rb

Create new `SECRET_KEY_BASE` and `APP_DATABASE_PASSWORD`

    rake secret

Update database with new password

    sudo -u postgres psql -c "ALTER USER rails PASSWORD 'app-database-password';"

Give rails user full access

    chown -R rails source

Use the rails user for whatever you can

    su - rails

Install dependencies, ensure unicorn gem is included

    RAILS_ENV=production bundle install
    RAILS_ENV=production bundle clean --force

Setup the database

    RAILS_ENV=production rails db:create
    RAILS_ENV=production rails db:migrate
    RAILS_ENV=production rails db:seed

Precompile the assets (may need to be done elsewhere)

    RAILS_ENV=production rails assets:precompile

Replace example.com with your website

    cp /etc/nginx/sites-available/rails.example /etc/nginx/sites-available/rails
    vim /etc/nginx/sites-available/rails

# Lets Encrypt

Replace example.com with your website

    cp usr/local/etc/le-renew-webroot.ini.example usr/local/etc/le-renew-webroot.ini
    vim /usr/local/etc/le-renew-webroot.ini

Run the script to genereate the SSL Cert

    sudo /usr/local/etc/le-renew-webroot

Uncomment the last line to turn on the cron job

    crontab -e

Replace the default nginx config

    cp /etc/nginx/sites-available/rails-ssl.example /etc/nginx/sites-available/rails
    vim /etc/nginx/sites-available/rails

# Restart

    service unicorn restart
    service nginx reload

# Logs

    tail -f /var/log/unicorn/unicorn.log

# Additional Info

https://www.digitalocean.com/community/tutorials/how-to-use-the-ruby-on-rails-one-click-application-on-digitalocean
https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-14-04
