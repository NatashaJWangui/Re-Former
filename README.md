# Re-Former

This is part of the Forms Project in The Odin Project's Ruby on Rails Curriculum. Find it at https://www.theodinproject.com

A comprehensive project demonstrating the evolution of form building in Rails - from bare HTML forms to modern Rails helpers.

## Features

- **Pure HTML Forms**: Built from scratch with manual CSRF protection
- **Rails form_tag Helpers**: Semi-automated form building with Rails helpers
- **Modern form_with**: Current Rails best practices for form creation
- **User Management**: Create and edit users with full validation
- **Error Handling**: Comprehensive validation error display
- **CSRF Protection**: Understanding Rails security mechanisms
- **Turbo Integration**: Modern Rails form submission handling

## Technologies Used

- **Ruby**: 3.4.4
- **Rails**: 8.0.2
- **Database**: SQLite3 (development)
- **Frontend**: HTML, ERB templates, Rails form helpers
- **Security**: CSRF tokens, strong parameters

## Form Evolution Demonstrated

### 1. Pure HTML Form
```html
<form method="post" action="/users" accept-charset="UTF-8" data-turbo="false">
  <input type="hidden" name="authenticity_token" value="<%= form_authenticity_token %>">
  <input type="text" name="user[username]">
  <!-- ... -->
</form>
```

### 2. Rails form_tag Helpers
```erb
<%= form_tag("/users", method: :post, local: true) do %>
  <%= label_tag(:username, "Username:") %>
  <%= text_field_tag(:username) %>
  <%= submit_tag("Create User") %>
<% end %>
```

### 3. Modern form_with
```erb
<%= form_with model: @user, local: true do |form| %>
  <%= form.label :username %>
  <%= form.text_field :username, placeholder: "Enter username" %>
  <%= form.submit %>
<% end %>
```

## Installation

### Prerequisites
- Ruby 3.0 or higher
- Rails 7.0 or higher
- SQLite3
- Git

### Setup
1. **Clone the repository**
   ```bash
   git clone https://github.com/NatashaJWangui/Re-Former.git
   cd re-former
   ```

2. **Install dependencies**
   ```bash
   bundle install
   ```

3. **Set up the database**
   ```bash
   rails db:migrate
   ```

4. **Start the server**
   ```bash
   rails server
   ```

5. **Visit the application**
   - New User Form: `http://localhost:3000/users/new`
   - Edit User Form: `http://localhost:3000/users/1/edit` (after creating a user)

## Project Structure

```
re-former/
├── app/
│   ├── controllers/
│   │   └── users_controller.rb    # User creation and editing logic
│   ├── models/
│   │   └── user.rb                # User model with validations
│   └── views/
│       └── users/
│           ├── new.html.erb       # New user form (all 3 versions)
│           └── edit.html.erb      # Edit user form
├── config/
│   └── routes.rb                  # User routes (new, create, edit, update)
├── db/
│   └── migrate/                   # User model migration
└── README.md
```

## User Model

### Attributes
- **username**: String (required)
- **email**: String (required)
- **password**: String (required)

### Validations
```ruby
class User < ApplicationRecord
  validates :username, presence: true
  validates :email, presence: true
  validates :password, presence: true
end
```

## Routes

| HTTP Method | Path | Controller#Action | Purpose |
|-------------|------|------------------|---------|
| GET | /users/new | users#new | Display new user form |
| POST | /users | users#create | Create new user |
| GET | /users/:id/edit | users#edit | Display edit user form |
| PATCH/PUT | /users/:id | users#update | Update existing user |

## Controller Implementation

### Strong Parameters
```ruby
private

def user_params
  params.require(:user).permit(:username, :email, :password)
end
```

### Error Handling
```ruby
def create
  @user = User.new(user_params)
  
  if @user.save
    redirect_to new_user_path
  else
    render :new, status: :unprocessable_entity
  end
end
```

## Key Learning Concepts

### Form Building Evolution
1. **HTML Forms**: Understanding the fundamentals
   - Form attributes (`method`, `action`, `accept-charset`)
   - Input types and naming conventions
   - Manual CSRF token inclusion

2. **Rails form_tag**: Introduction to Rails helpers
   - Automatic CSRF protection
   - Helper methods for common inputs
   - Cleaner, more maintainable code

3. **Rails form_with**: Modern Rails approach
   - Model-aware form building
   - Automatic route detection
   - Built-in error handling integration

### Security Features
- **CSRF Protection**: Cross-Site Request Forgery prevention
- **Strong Parameters**: Whitelisting allowed parameters
- **Turbo Integration**: Modern Rails form submission

### Data Validation
- **Presence Validation**: Required field validation
- **Error Display**: User-friendly error messages
- **Form State Preservation**: Maintaining form data on validation failure

## Usage Examples

### Creating a New User
1. Navigate to `/users/new`
2. Fill in username, email, and password
3. Submit form
4. See validation errors if data is invalid
5. Redirected to new form if successful

### Editing an Existing User
1. Navigate to `/users/:id/edit`
2. Form pre-populated with existing user data
3. Modify fields as needed
4. Submit to update user
5. See validation errors if data is invalid

### Form Comparison
The project contains three versions of the same form:
- **HTML Version**: For understanding fundamentals
- **form_tag Version**: For learning Rails helpers
- **form_with Version**: For modern Rails development

## Testing

### Manual Testing Checklist
- [ ] Can create user with valid data
- [ ] Cannot create user with empty fields
- [ ] Validation errors display properly
- [ ] Can edit existing user
- [ ] Form fields pre-populate on edit
- [ ] CSRF protection works (no token = error)
- [ ] Strong parameters prevent unauthorized fields

### Console Testing
```ruby
# Create users programmatically
User.create(username: "test", email: "test@example.com", password: "password")

# Test validations
invalid_user = User.new
invalid_user.valid?  # => false
invalid_user.errors.full_messages
```

## Security Considerations

### CSRF Protection
- All forms include authenticity tokens
- Turbo requests automatically include CSRF headers
- Manual forms require `form_authenticity_token`

### Parameter Security
- Strong parameters prevent mass assignment
- Only whitelisted attributes are permitted
- Sensitive data (passwords) are filtered in logs

## Common Issues & Troubleshooting

### CSRF Token Errors
```
ActionController::InvalidAuthenticityToken
```
**Solution**: Ensure CSRF token is included in forms or meta tags are present

### Strong Parameter Errors
```
ActiveModel::ForbiddenAttributesError
```
**Solution**: Use strong parameters with `permit()` method

### Validation Failures
**Solution**: Check model validations and ensure all required fields are provided

### Turbo Issues
**Solution**: Use `local: true` for traditional form submission or properly handle Turbo responses

## Future Enhancements

Potential improvements to explore:
- **Advanced Validations**: Email format, password strength
- **Flash Messages**: Success/failure notifications
- **Styling**: CSS/Bootstrap integration
- **AJAX Forms**: Turbo frame integration
- **File Uploads**: Adding avatar/image fields
- **Nested Forms**: Handling complex associations
- **Form Objects**: Extracting form logic from controllers

## Learning Resources

- [Rails Guides - Action View Form Helpers](https://guides.rubyonrails.org/form_helpers.html)
- [Rails API - form_with](https://api.rubyonrails.org/classes/ActionView/Helpers/FormHelper.html#method-i-form_with)
- [Rails Guides - Strong Parameters](https://guides.rubyonrails.org/action_controller_overview.html#strong-parameters)
- [Rails Security Guide](https://guides.rubyonrails.org/security.html)

## Contributing

1. Fork the project
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is for educational purposes

## Acknowledgments

- The Odin Project for the comprehensive forms curriculum
- Rails community for excellent documentation and helpers
- All developers contributing to Rails form handling improvements

---

**Project completed as part of The Odin Project - Ruby on Rails Course**

