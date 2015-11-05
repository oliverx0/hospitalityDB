# User params when accessing database from view form
def user_params
    params.require(:user).permit(:email, :name, :password, :password_confirmation)
end

# Example
<%= form_for (@user) do |f| %>
    <%= render 'shared/error_messages' %>

  <%= f.label :name %>
  <%= f.text_field :name, class: 'form-control' %>

  <%= f.label :email %>
  <%= f.text_field :email, class: 'form-control' %>

  <%= f.label :password %>
  <%= f.password_field :password, class: 'form-control' %>

  <%= f.label :password_confirmation %>
  <%= f.password_field :password_confirmation, class: 'form-control' %>

  <%= f.submit "Create my account", class: "btn btn-primary" %>
<% end %>

# Get params otherwise (directly)
params[:session][:email]

#Example
<%= form_for(:session, url: login_path) do |f| %>
   <%= f.label :email %>
   <%= f.email_field :email, class: 'form-control' %>

   <%= f.label :password %>
   <%= f.password_field :password, class: 'form-control' %>

   <%= f.submit "Log In", class: 'btn btn-primary' %>
<% end %>

# Example access in a controller
def create
    @user = User.new(user_params)
    if @user.save
      flash[:success] = "Welcome to the Sample App!"
      redirect_to @user
    else
      render 'new'
    end
end




# To generate a new model
rails generate model User name:string email:string

# To add a new column to model
rails generate migration MIGRATION_NAME_to_MODEL_NAME(plural) COLUMN_NAME:type

bundle exec rake db:migrate

# To add a new object to the db
User.new(name: 'Oliver', email: 'ojh22@©ornell.edu')

# To see models attributes
Model.columns.collect { |c| "#{c.name} (#{c.type})" }

# To see if user exists in database
User.new.new_record?
User.first.new_record?

# To drop a table
rails c

# To delete all content in a table
User.delete_all



# Encrypt with MD5 algorithm
require 'digest'
hash_val = Digest::MD5::hexdigest('oliver')

# Encrypt with BCrypt algorithm
require 'bcrypt'
hash_val = BCrypt::Password.create('oliver', cost: 5)
hash_val == 'oliver' # => true

# Example using min cost for password digest
def self.digest(string)
    cost = ActiveModel::SecurePassword.min_cost ? BCrypt::Engine::MIN_COST : BCrypt::Engine.cost
    BCrypt::Password.create(string, cost)
 end


TO DEPLOY: http://dennissuratna.com/rails-deployment-aws1/

From local: git add., git commit -m, git push

From web:

 - rbenv sudo passenger stop  -p80
 - bundle install
 - git checkout .
 - git pull
 - rbenv rehash
 - //rake db:migrate RAILS_ENV=production
 - rbenv sudo rake assets:precompile RAILS_ENV=production
 - rbenv sudo passenger start -p80 -d -e production

Note: this works because env has been setup via rbenv vars
# If the vies URL is users/:id/...
paramas[:id] # => retrieves id
