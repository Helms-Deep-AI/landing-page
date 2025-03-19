# Enkrasia Landing Page

This is the landing page for Enkrasia, a platform that helps families align their digital experiences with their core values.

## Form Submission to Supabase

The signup form on this site submits data to a Supabase database. To set this up, follow these steps:

### Setting up Supabase

1. Create a Supabase account at [Supabase](https://supabase.com/) if you don't have one already.

2. Create a new project in Supabase and note down your project URL and API key (found in Project Settings > API).

3. In your Supabase project, create a new table called `submissions` with the following columns:
   - `id` (type: uuid, primary key)
   - `email` (type: text)
   - `religion` (type: text)
   - `values` (type: text[])
   - `content_to_avoid` (type: text[])
   - `topics_to_avoid` (type: text[])
   - `searches` (type: text[])
   - `timestamp` (type: timestamp with time zone)

4. Create a `config.js` file in the root directory of your project with the following content:

```javascript
// Supabase configuration
const SUPABASE_URL = 'https://your-project-url.supabase.co';
const SUPABASE_API_KEY = 'your-supabase-api-key';
```

5. Replace `'https://your-project-url.supabase.co'` with your actual Supabase project URL and `'your-supabase-api-key'` with your actual Supabase API key.

6. Make sure to include the `config.js` file in your HTML files before any scripts that use the Supabase client:

```html
<script src="config.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
```

### Setting up Row-Level Security (Optional but Recommended)

1. In your Supabase dashboard, go to the Authentication section and enable "Anonymous Key" if you want to allow unauthenticated submissions.

2. Go to the SQL Editor and run the following query to set up Row-Level Security (RLS) policies:

```sql
-- Enable RLS on the submissions table
ALTER TABLE submissions ENABLE ROW LEVEL SECURITY;

-- Create a policy that allows anonymous inserts
CREATE POLICY "Allow anonymous inserts" ON submissions
FOR INSERT WITH CHECK (true);

-- If you want to restrict who can view the submissions, add a policy like this:
CREATE POLICY "Only authenticated users can view submissions" ON submissions
FOR SELECT USING (auth.role() = 'authenticated');
```

### Testing the Form Submission

1. Open the signup page in your browser and fill out the form.

2. Submit the form and check your Supabase table to see if the data was added correctly.

3. Check the browser console (F12 > Console) for any error messages during form submission.

## Updating the Website

After setting up Supabase and creating the config.js file, make sure all HTML files that need to interact with Supabase include the necessary script tags.

## File Structure

- `index.html`: The main landing page
- `about.html`: Information about Enkrasia
- `products.html`: Information about Enkrasia's products
- `signup.html`: The signup form
- `config.js`: Configuration file with Supabase credentials
- `README.md`: This file

## Development

To work on this project locally:

1. Clone the repository
2. Create a `config.js` file with your Supabase credentials as described above
3. Open the HTML files in your browser to test

## Security Note

The `config.js` file contains sensitive API keys. Make sure to:

1. Add `config.js` to your `.gitignore` file to prevent it from being committed to your repository
2. Use environment variables in production environments
3. Consider implementing proper authentication for production use