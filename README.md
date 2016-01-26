# Setup

Pull for updates to this file

Use the rails user for everything you can

    su - rails

Clone the repo into the source folder

    git clone https://github.com/tomprats/example.git source 

Update the database config

    mv database.yml source/config/database.yml

Create and add ENV variables

    vim source/config/local_variables.rb

Create new `SECRET_KEY_BASE` and `APP_DATABASE_PASSWORD`

    rake secret
    vim /etc/default/unicorn

Update database with new password

    sudo -u rails psql -c "ALTER USER rails PASSWORD 'new-password';"

Source the unicorn config

    . /etc/default/unicorn

Install dependencies, ensure unicorn gem is included

    RAILS_ENV=production bundle install
    RAILS_ENV=production bundle clean --force

Setup the database

    RAILS_ENV=production rails db:create
    RAILS_ENV=production rails db:migrate
    RAILS_ENV=production rails db:seed

Precompile the assets (may need to be done elsewhere)

    RAILS_ENV=production rails assets:precompile

Give rails user full access

    chown -R rails source

Replace example.com with your website

    vim /etc/nginx/sites-enabled/rails

# Lets Encrypt

Replace example.com with your website

    vim /usr/local/etc/le-renew-webroot.ini

Run the script to genereate the SSL Cert

    sudo le-renew-webroot

Uncomment the last line to turn on the cron job

    crontab -e

# Restart

    service unicorn restart
    service nginx reload

# Logs

    tail -f /var/log/unicorn/unicorn.log

# Additional Info

https://www.digitalocean.com/community/tutorials/how-to-use-the-ruby-on-rails-one-click-application-on-digitalocean
