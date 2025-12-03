# Flashcard Data

This directory contains the flashcard data in JSON format that can be hosted on GitHub and fetched by the app.

## Files

- `cards.json` - All flashcard data
- `decks.json` - All deck definitions

## Setting Up GitHub Hosting

### Step 1: Create a GitHub Repository

1. Go to [GitHub](https://github.com) and create a new repository
2. Name it something like `bekhooon-data` or `persian-flashcards-data`
3. Make it **public** (so the app can access it without authentication)

### Step 2: Upload the Data Files

1. Upload the `cards.json` and `decks.json` files to your repository
2. You can put them in a `data/` folder or in the root directory
3. Commit and push the files

### Step 3: Configure the App

1. Open `services/api.ts`
2. Update these constants:
   ```typescript
   const GITHUB_USERNAME = 'your-github-username';
   const GITHUB_REPO = 'your-repo-name';
   const GITHUB_BRANCH = 'main'; // or 'master'
   const GITHUB_DATA_PATH = 'data'; // or '' if files are in root
   ```

### Step 4: Test

The app will automatically:
- Try to fetch data from GitHub first
- Fall back to local data if GitHub fetch fails
- Cache the data for better performance

## Data Structure

### Simplified Structure (No cardIds!)

The data structure has been simplified to make it easier to add/remove words:

- **Decks** (`decks.json`): Only contain deck metadata (id, name, category, description, order)
- **Cards** (`cards.json`): Each card has:
  - `id`: Unique identifier (e.g., "alef-1" or "alef-آب")
  - `word`: The Persian word
  - `deckId`: Which deck this card belongs to
  - `image`: Emoji or image identifier

**Key Benefit**: To add a word, just add it to `cards.json` with the correct `deckId`. No need to update the deck's `cardIds` array!

### Example Card Structure

```json
{
  "id": "alef-آب",
  "word": "آب",
  "deckId": "alef",
  "image": "💧"
}
```

### Example Deck Structure

```json
{
  "id": "alef",
  "name": "ا",
  "category": "ا",
  "description": "کلمات با حرف ا",
  "isCustom": false,
  "order": 1
}
```

## Adding/Removing Words

### To Add a Word:

1. Open `cards.json`
2. Add a new card object:
   ```json
   {
     "id": "alef-نوین",
     "word": "نوین",
     "deckId": "alef",
     "image": "✨"
   }
   ```
3. The ID can be any unique string (e.g., `deckId-word` format)
4. Commit and push to GitHub
5. Done! No need to update `decks.json`

### To Remove a Word:

1. Open `cards.json`
2. Remove the card object
3. Commit and push to GitHub
4. Done!

## Updating Data

To update the flashcard data:

1. Edit the data in `constants/defaultDecks.ts` (for local development)
2. Or directly edit `cards.json` and `decks.json` (for server updates)
3. Run the script to regenerate JSON files (if using constants):
   ```bash
   node scripts/generate-json.js
   ```
4. Commit and push the updated JSON files to GitHub
5. The app will fetch the new data on next launch (or call `api.clearCache()` to force refresh)

## Example GitHub URLs

If your repository is `https://github.com/username/bekhooon-data`:
- Cards: `https://raw.githubusercontent.com/username/bekhooon-data/main/data/cards.json`
- Decks: `https://raw.githubusercontent.com/username/bekhooon-data/main/data/decks.json`

