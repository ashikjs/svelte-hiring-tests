<script lang="ts">
  import type {FormErrors, UserFormData} from "src/app";

  // TODO: Implement form state management
  let registrationFormData: UserFormData = {
    firstName: '',
    lastName: '',
    email: '',
    password: ''
  }
  let successMessage = ''


  // TODO: Implement form validation
  let registrationFormErrors: FormErrors = {}

  function isValidUserData(user: UserFormData) {
    let error: FormErrors = {}
    if (!user.firstName) error.firstName = 'First Name is required'
    if (!user.lastName) error.lastName = 'Last Name is required'
    if (!user.email) {
      error.email = 'Email is required'
    } else if (!/^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$/.test(user.email)) {
      error.email = 'Enter a valid email'
    }
    if (!user.password) {
      error.password = 'Password is required'
    } else if (user.password?.length < 6) {
      error.password = 'Password must be at least 6 characters long'
    }

    registrationFormErrors = error

    return Object.keys(error).length === 0
  }

  function clearSuccessMessage() {
    successMessage = ''
  }

  // TODO: Implement submit handler
  function registrationFormSubmit(event: Event) {
    event.preventDefault()

    if (isValidUserData(registrationFormData)) {
      successMessage = `
            <div class="success-message">
                <h3>Successfully submitted:</h3>
                <p>First Name: ${registrationFormData.firstName}</p>
                <p>Last Name: ${registrationFormData.lastName}</p>
                <p>Email: ${registrationFormData.email}</p>
            </div>
            `
      registrationFormData = {firstName: '', LastName: '', email: '', password: ''}
      registrationFormErrors = {}
    }
  }

  // TODO: Implement success state management
</script>

<div class="form-container">
    <form on:submit|preventDefault={registrationFormSubmit}>
        <div class="form-group">
            <label for="firstName">First Name</label>
            <input
                    id="firstName"
                    type="text"
                    placeholder="Enter your first name"
                    bind:value={registrationFormData.firstName}
                    class:error={registrationFormErrors.firstName}
                    on:input={clearSuccessMessage}
            />
            {#if registrationFormErrors.firstName}
                <span class="error-message">{registrationFormErrors.firstName}</span>
            {/if}
        </div>

        <div class="form-group">
            <label for="lastName">Last Name</label>
            <input
                    id="lastName"
                    type="text"
                    placeholder="Enter your last name"
                    bind:value={registrationFormData.lastName}
                    class:error={registrationFormErrors.lastName}
                    on:input={clearSuccessMessage}
            />
            {#if registrationFormErrors.lastName}
                <span class="error-message">{registrationFormErrors.lastName}</span>
            {/if}
        </div>

        <div class="form-group">
            <label for="email">Email</label>
            <input
                    id="email"
                    type="email"
                    placeholder="Enter your email"
                    bind:value={registrationFormData.email}
                    class:error={registrationFormErrors.email}
                    on:input={clearSuccessMessage}
            />
            {#if registrationFormErrors.email}
                <span class="error-message">{registrationFormErrors.email}</span>
            {/if}
        </div>

        <div class="form-group">
            <label for="password">Password</label>
            <input
                    id="password"
                    type="password"
                    placeholder="Enter your password"
                    bind:value={registrationFormData.password}
                    class:error={registrationFormErrors.password}
                    on:input={clearSuccessMessage}
            />
            {#if registrationFormErrors.password}
                <span class="error-message">{registrationFormErrors.password}</span>
            {/if}
        </div>

        <button type="submit" class="submit-button">Submit</button>
    </form>

    <!-- TODO: Add success message section here -->
    {#if successMessage}
        <div class="success-message" on:click={clearSuccessMessage}>{@html successMessage}</div>
    {/if}
</div>

<style>
    .form-container {
        max-width: 480px;
        margin: 0 auto;
        padding: 2rem;
        background-color: white;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }

    .form-group {
        margin-bottom: 1.5rem;
    }

    label {
        display: block;
        margin-bottom: 0.5rem;
        font-weight: 500;
        color: #374151;
    }

    input {
        width: 100%;
        padding: 0.75rem;
        border: 1px solid #D1D5DB;
        border-radius: 6px;
        font-size: 1rem;
        transition: border-color 0.15s ease-in-out;
    }

    input:focus {
        outline: none;
        border-color: #3B82F6;
        box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
    }

    input::placeholder {
        color: #9CA3AF;
    }

    input.error {
        border-color: #EF4444;
    }

    .error-message {
        display: block;
        margin-top: 0.5rem;
        font-size: 0.875rem;
        color: #EF4444;
    }

    .submit-button {
        width: 100%;
        padding: 0.75rem 1.5rem;
        background-color: #3B82F6;
        color: white;
        border: none;
        border-radius: 6px;
        font-size: 1rem;
        font-weight: 500;
        cursor: pointer;
        transition: background-color 0.15s ease-in-out;
    }

    .submit-button:hover {
        background-color: #2563EB;
    }

    .submit-button:focus {
        outline: none;
        box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.5);
    }

    .submit-button:disabled {
        background-color: #9CA3AF;
        cursor: not-allowed;
    }

    .success-message {
        margin-top: 1.5rem;
        padding: 1rem;
        background-color: #EDF7ED;
        border: 1px solid #C8E6C9;
        border-radius: 6px;
        color: #1B5E20;
    }
</style>
