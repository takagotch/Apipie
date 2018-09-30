### Apipie
---
https://github.com/Apipie/apipie-rails

```ruby
config.disqus_shortname = "MyProjectDoc"

describe "These are edge cases" do
  it "some test", :show_in_doc do
  end
end
context "These are edge cases" do
  it "Cant authenticate" do
  end
  it "record not found" do
  end
end

RSpec.configure do |config|
  config.treat_symbols_as_metadata_keys_with_true_values = true
  conifg.filter_run :show_in_doc => true if ENV['APIPIE_RECORD']
end

```

```
APIPIE_RECORD=examples rake test:functionals
APIPIE_RECORD=examples rails server

--- !omap
  - announcements#index:
    - title: This is a custom title for this example
    - verb: :GET
    - path: /api/blabla/1
    - versions:
      - '1.0'
    - query:
    - request_data:
    - response_data:
    ...
    - code: 200
    - show_in_doc: 1
    - recorded: true
```

```sh
APIPIE_RECORD=params rake test:functionals
APIPIE_RECORD=params rails server
```

```ruby
"Apipie-Checksum"=>"xxxxxxxxxx"
require 'apipie/middleware/checksum_in_headers'
config.middleware.use "Apipie::Middleware::ChecksumInHeaders"
Apipie.configuration.update_checksum = true

param :do_something, Boolean, :desc => "take an action", :required => false, :default_value => false

Apipie.configure do |config|
  config.languages = ['en', 'cs']
  config.default_locale = 'en'
  config.locale = lambda { |loc| loc ? FastGettext.set_locale(loc) : FastGettext.locale }
  config.translate = lambda do |str, loc|
    old_loc = FastGettext.locale
    FastGettext.set_locale(loc)
    trans = _(str)
    FastGettext.set_locale(old_loc)
    trans
  end
end

api :GET, "/users/:id", N_("Show user profile")
param :session, String, :desc => N_("user is logged in"), :required => true

Apipie.configure do |config|
  config.languages = ['en', 'cs']
  config.default_locale = 'en'
  config.locale = lambda { |loc| loc ? I18n.locale = loc : I18n.locale }
  config.translate = labda do |str, loc|
    return '' if str.blank?
    I18n.t str, locale: loc, scope: 'doc'
  end
end

api :GET, "/users/:id", "show_user_profile"
params :session, String, :desc => "user_is_logged_in", :required => true

class Textile
  def initialize
    require 'RedCloth'
  end
  def to_html(text)
    RedCloth.new(text).to_html
  end
end

resource_description do
  api_version "1", "2"
end

api :GET, "/api/users/", "List: users"
api_version "1"
def index
end
api :GET, "/api/users/", "List: users", :deprecated => true

/apipie/1/users/index
/apipie/2/users/index


param :comments, Array, :desc => "User comments" do
  param :name, String, :desc => "Name of the comment", :required => true
  param :comment, String, :desc => "Full comment", :required => true
end

param :id, Integer, :desc => "Company ID"

class IntegerValidator < Apipie::Validator::BaseValidator
  def initialize(param_description, argument)
    super(param_description)
    @type = argument
  end
  def validate(value)
    return false if value.nil?
    !!(value.to_s =~ /^[-+]?[0-9]+$/)
  end
  def self.build(param_descirption, argument, options, block)
    if argument == Integer || argument == Fixnum
      self.new(params_description, argument)
    end
  end
  def description
    "Must be #{@type}."
  end  
end


param: :latitude, :decimal, :desc => "Georaphic latitude", :required => true
param: :longitude, :decimal, :desc => "Geographic longitude", :required => true

param :things, Array
param :hits, Array, of: Integer
param :color, Array, in: ["red", "green", "blue"]
param :colors, Array, in: -> { Color.all.pluk(:name) }

param :user, nil
def destroy
end

params :user, Hash, :desc => "User info" do
  param :username, String, :desc => "Username for login", :required => true
  param :password, String, :desc => "Password for login", :required => true
  param :membership, ["standard", "premium"], :desc => "User membership"
end

param :proc_param, lambda {|val|
  val == "param value" ? true : "The only good value is 'param value'."
}, :desc => "proc validator"


param :regexp_param, /^[0-9]* years/, :desc => "regexp param"

param :session, String, :desc => "user is logged in", :required => true
param :facts, Hash, :desc => "Additional optional facts about the user"


before_action :apipie_validations

def process_value(value)
  value ? value.split(',') : []
end

class MyFormatter < Apipie::RoutesFormatter
  def format_path(route)
    super.gsub(/\(.*?\)/, '').gsub('//', '')
  end
end
Apipie.configure do |config|
  config.routes_formatter = MyFormatter.new
end

config.locale = lambda { |loc| loc ? FastGettext.set_locale(loc) : FastGettext.locale }

Apipie.configure do |config|
  config.app_name = "Test app"
  config.copyright = "&copy; 2018 takagotch"
  config.doc_base_url = "/apidoc"
  config.api_base_url = "/api"
  config.validate = false
  config.markup = Apipie::Markup::Markdown.new
  config.reload_controlers = Rails.env.development?
  config.api_controllers_matcher = File.join(Rails.root, "app", "controllers", "**", "*.rb")
  config.api_routes = Rails.application.routess
  config.app_info["1.0"] = "
  text...
  "
  config.authenticate = Proc.new do
    authenticate_or_request_with_http_basic do |username, password|
      username == "test" && password == "supersecretpassword"
    end
  end
  config.authorize = Proc.new do |controller, method, doc|
    !method 
  end
end



require 'apipie/rspec/response_validation_helper'
RSpec.describe MyController, :type => controller, :show_in_doc => true do
  descirbe "GET stuff with response validation" do
    render_views
    auto_validate_rendered_views
    it "does something"do
      get :index, {format: :json}
    end
    it "does something else" do
      get :another_index, {format: :json}
    end
  end
  descirbe "GET stuff without response validation" do
    it "does something" do
      get :index, {format: :json}
    end
    it "does something else" do
      get :another_index, {format: :json}
    end
  end
end


require 'apipie/rspec/response_validation_helper'
RSpec.describe MyController, :type => :controller, :show_in_doc => true do
  describe "GET stuff with response validation" do
    render_views
    it "does something" do
      response = get :index, {format: :json}
      expect(response).to match_declared_responses
    end
  end
end



module Concerns
  module OauthConcern
    extend Apipie::DSL::Concern
    update_api(:create, :update) do
      param :user, Hash do
        param :oauth, String, :desc => 'oauth param'
      end
    end
  end
end


#user_module.rb
module UserModule
  extend Apipie::DSL::Concern
  api :GET, '/:controller_path', 'List :resource_id'
  def index
  end
  api! 'Show a :resource'
  def show
  end
  api :POST, '/:resource_id', "Create a :resource"
  param :concern, Hash, :required => true
    param :name, String, 'Name of a :resource'
    param :resource_type, ['standard', 'vip']
  end
  def create
  end
  api :GET, '/:resource_id/:custom_subst'
  def custom
  end
end


class PetWithMeasurements
  def self.describe_own_properties
    [
      Apipie::prop(:pet_name, 'string', {:description => 'Name of pet', :required => false}),
      Apipie::prop('animal_type', 'string', {:description => 'Type of pet', :values => ["dog", "cat", "iguana", "kangaroo"]}),
      Apipie::prop(:pet_measurements, 'object', {}, [
        Apipie::prop(:weight, 'number', {:description => "Weight in pound"}),
        Apipie::prop(:height, 'number', {:description => "Height in puund"}),
        Apipie::prop(:num_legs, 'number', {:description => "Number of legs", :required => false }),
        Apipie::additional_properties(false)
      ])
    ]
  end
end
class PetWithManyMeasurements
  def self.describe_own_properties
    [
      Apipie::prop(:pet_name, 'string', {:description => 'Name of pet', :required => false}),
      Apipie::prop(:many_pet_measurements, 'object', {is_array: true}, [
        Apipie::prop(:weight, 'number', {:description => "Weight in pounds"}),
        Apipie::prop(:height, 'number', {:description => "Height in inches"})
      ])
    ]
  end
end


Apipie::prop(<property-name>, <property-type>, <option-hash> [, <array of sub-properties>])

class Pet
  def self.describe_own_properties
    [
      Apipie::prop(:pet_name, 'string', {:description => 'Name of pet', :required => false})
      Apipie::prop(:animal_type, 'string', {:description => 'Type of pet', :values => ["dog", "cat", "iguana", "kangaroo"]})
      Apipie::additional_properties(false)
    ]
  end
  def json
    JSON({:pet_name => @name, :animal_type => @type})
  end
end
class PetsController
  api :GET, "/index", "Get all pets"
  returns :array_of => Pet
  def index
  end
end




def_param_group :user_record
  param :name, String
  param :force_update, [true, false], :only_in => :request
  property :last_login, String
end
api :POST, "/users", "Create a user"
param_group :user_record
def index
end
api :GET, "/users", "Create a user"
returns :array_of => :user_record
def index
end

property :example, :array_of => Hash do
  property :number1, Integer
  property :namber2, Integer
end

api :GET, "/pets/:id/extra_info", "Get extra information about a pet"
  returns :desc => "Found a pet" do
    param_group :pet
    property 'pet_histroy', Hash do
      param_group :pet_histroy
    end
  end
  returns :code => :unprocessable_entity, :desc => "Fleas were discovered on the pet" do
    param_gorup :pet
    property :num_fleas, Integer, :desc => "Number of fleas on this pet"
  end
  def show_extra_info
  end
  

def_param_group :get do
  property :pet_name, String, :desc => "Name of pet"
  property :animal_type, ['dog', 'cat', 'iguana', 'kangaroo'], :desc => "Type of pet"
end
api :GET, "/pets/:id", "Get a pet record"
returns :pet, :desc => "The pet"
def show_detailed
  render JSON({:pet_name => "Skippie", :animal_type => "kangaroo"})
end

api :GET "/pets/:id/with-extra-details", "Get a detailed pet record"
returns :code => 200, :desc => "Detailed info about the pet" do
  param_group :pet
  property :num_legs, Integer, :desc => "How many legs the pet has"
end
def show
  render JSON({:pet_name => "Barkie", :animal_type => "iguana", legs => 4})
end

api :GET, "/pets", "Get all pet records"
returns :array_of => :pet, :code => 200, :desc => "All pets"
def index
  render JSON([ {:pet_name => "Skippie", :animal_type => "kangaroo"},
                {:pet_name => "Woofie", :animal_type => "cat"}])
end


returns <param-group-name> [, :code => <number>|<http-response-code-symbol>] [, :desc => <human-readable description>]

returns :code => <number>|<http-response-code-symbol> [, :desc => <human-redable description>] do
end

returns :array_of => <param-group-name> [, :code => <number>|<http-response-code-symbol>] [, :desc => <human-readable description>]


def_param_group :user do
  param :user, Hash, :action_aware => true do
    param :name, String, :required => true
    param :description, String
  end
end

api :POST, "/users", "Create an user"
param_group :user
def create
end

api :PUT, "/users/admin", "Create an admin"
param_group :user, :as => :create
def create_admin
end

api :PUT, "/users/:id", "Update an user"
param_group :user
def update
end

# v1/users_controller.rb
def_param_group :address do
  param :street, String
  param :number, Integer
  param :zip, String
end

def_param_group :user do
  param :user, Hash do
   param :name, String, "Name of the user"
   param_group :address
  end
end

api :POST, "/users", "Create an user"
param_group :user
def create
end

api :PUT, "/users/:id", "Update an user"
param_gorup :user
def update
end

api :PUT, "/users/:id", "Update an user"
param_group :user
def update
end

# v2/users_controller.rb
api :POST, "/users", "Create an user"
param_group :user, V1::UsersController
def create
end


param :user, Hash, :desc => "User info" do
  param :username, String, :desc => "Username for login", :required => true
  param :password, String, :desc => "Password for login", :required => true
  param :membership, ["standard", "premium"], :desc => "User membership"
  param :admin_override, String, :desc => "Not shown in documentation", :show => false
  param :ip_address, String, :desc => "IP address", :required => true, "missing_message => lambda { I18n.t("ip_address.required")}
end
def create
end



api!
def index
end

api :GET, "/users/:id", "Show user profile"
show false
error :code => 401, :desc => "Unauthorized"
error :code => 404, :desc => "Not Found", :meta => {:anything => "you can think of"}
param :session, String, :desc => "user is logged in", :required => true
param :regexp_param, /^[0-9]* years/, :desc => "regexp param"
param :array_param, [100, "one", "two", 1, 2], :desc => "array validator"
param :boolean_param, [true, false], :desc => "array validator with boolean"
param :proc_param, lambda { |val|
  val == "param value" ? true " "The only good value is 'param value'."
}, :desc => "proc validator"
param :param_with_metadata, String, :desc => "", :meta => [:your, :custom, :metadata]
returns :code => 200, :desc => "a successful response" do
  property :value1, String, :desc => "A string value" do
  property :value2, Integer, :desc => "An integer value"
  property :value3, Hash, :desc => "An object" do
    property :enum1, ['v1', 'v2'], :desc => "One of 2 possible string values"
  end
end
description "method description"
formats ['json', 'jsonp', 'xml']
meta :message => "Some very important info"
example " user': {...}"
see "users#showme", "link description"
see :link => "users#update", :desc => "another link description"
def show
end


resoruce_description do
  short 'Site members'
  formats ['jsop']
  param :id, FIxnum, :desc => "User ID", :required => false
  param :resource_param, Hash, :desc => 'param description for all methods' do
  end
  api_version "development"
  error 404, "Missing"
  error 500, "Server crasged for soem <%= reason %>", :meta => {:anything => "you can think of"}
  error :unprocessable_entitiy, "Could not save the entity."
  returns :code => 403 do
    property :reason, String, :desc => "Why this was forbidden"
  end
  meta :author => {:name => 'John', :surname => 'Doe'}
  deprecated false
  description <<-EOS
    == Long description
     Example resource for rest api documentation
     These can now be accessed in <tt>shared/header</tt> with:
       Headline: <%= headline %>
       First name: <%= person.first_name %>

     If you need to find out whether a certain local variable has been
     assigned a value in a particular render call, you need to use the
     following pattern:

     <% if local_assigns.has_key? :headline %>
        Headline: <%= headline %>
     <% end %>

    Testing using <tt>defined? headline</tt> will not work. This is an
    implementation restriction.

    === Template caching

    By default, Rails will compile each template to a method in order
    to render it. When you alter a template, Rails will check the
    file's modification time and recompile it in development mode.
  EOS
end





```





