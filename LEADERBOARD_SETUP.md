# SSOAR Leaderboard Setup Guide

## Setting up Supabase for the Leaderboard

### 1. Create a Supabase Project
1. Go to [supabase.com](https://supabase.com) and sign up/sign in
2. Click "New Project"
3. Choose your organization, enter a project name (e.g., "ssoar-leaderboard")
4. Enter a database password and select a region
5. Click "Create new project"

### 2. Create the Leaderboard Table
1. In your Supabase dashboard, go to the "SQL Editor"
2. Run this SQL query:

```sql
CREATE TABLE leaderboard (
  id BIGSERIAL PRIMARY KEY,
  player_name TEXT NOT NULL,
  score INTEGER NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Add an index for faster score queries
CREATE INDEX idx_leaderboard_score ON leaderboard(score DESC);

-- Enable Row Level Security
ALTER TABLE leaderboard ENABLE ROW LEVEL SECURITY;

-- Policy to allow anyone to read scores
CREATE POLICY "Anyone can view leaderboard" ON leaderboard
  FOR SELECT USING (true);

-- Policy to allow anyone to insert scores
CREATE POLICY "Anyone can submit scores" ON leaderboard
  FOR INSERT WITH CHECK (true);
```

### 3. Get Your Supabase Credentials
1. In your Supabase dashboard, go to "Settings" > "API"
2. Copy the "Project URL" and "anon public" key

### 4. Update Your Game Code
1. Open `index.html`
2. Find these lines near the top of the script section:
```javascript
const SUPABASE_URL = 'YOUR_SUPABASE_URL_HERE';
const SUPABASE_ANON_KEY = 'YOUR_SUPABASE_ANON_KEY_HERE';
```
3. Replace `YOUR_SUPABASE_URL_HERE` with your Project URL
4. Replace `YOUR_SUPABASE_ANON_KEY_HERE` with your anon public key

### 5. Test the Leaderboard
1. Play the game and get a game over
2. The leaderboard modal should appear after the game over screen
3. Enter a name and submit your score
4. Your score should appear in the leaderboard

## Features Added

- **Automatic Leaderboard Display**: Shows after game over with a 1-second delay
- **Score Submission**: Players can enter their name and submit scores
- **Top 10 Leaderboard**: Displays the top 10 scores with medals for top 3
- **Real-time Updates**: Leaderboard refreshes after score submission
- **Graceful Fallback**: If Supabase isn't configured, the game works normally without the leaderboard

## Security Notes

- The current setup allows anyone to submit scores (good for a simple game)
- For production use, you might want to add rate limiting or more sophisticated validation
- Consider adding a simple CAPTCHA or other anti-spam measures if needed

## Customization Options

You can customize the leaderboard by:
- Changing the number of top scores displayed (modify the `limit` in `getTopScores()`)
- Styling the leaderboard modal (modify the CSS in the HTML)
- Adding more fields to track (modify the database schema and submission logic)
- Adding score validation or filtering

## Troubleshooting

- **Leaderboard doesn't appear**: Check browser console for errors, ensure Supabase credentials are correct
- **Scores don't submit**: Verify the database table exists and RLS policies are set correctly
- **Network errors**: Check your Supabase project status and API keys