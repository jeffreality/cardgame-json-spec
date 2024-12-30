# Card Game JSON Specification

This JSON file provides a structured format to describe any card game. It includes core data such as the title, rules location, and decks with cards. Optional fields let you customize additional metadata and items placed on the table (score pads, tokens, etc.).

## Required Fields

1. **`version_str`**  
   - The version of the Card Game JSON Specification that this file can be validated against.  
   - **Example**: `"version_str": "1.0"`.
2. **`metadata.game_title_str`**  
   - A string that provides the name of your game.  
   - **Example**: `"game_title_str": "Go Math"`.
3. **`metadata.rules_url `**  
   - A secure url to an html page with the formatted rules of your game within a single scrollable page.  
   - **Example**: `"rules_url": "https://domain.com/game-rules.html"`.
4. **At Least One Deck**  
   - The `decks_list` array must have **at least one** deck object.  
   - That deck must have **at least one card** in its `cards_list`.

Without these three items (game title, rules, and at least one deck with one card), the JSON file is considered invalid.

## Interchangeable Media Fields (`_png`, `_jpg`, `_usdz`, etc.)

- Within the JSON, you will see fields like `"item_png"`, `"item_jpg"`, or `"item_usdz"`.  
- **Only one** of these should be used at a time for a given object. For example, if you have `"item_png"` you typically won’t also need `"item_usdz"`.  
- This allows flexibility for 2D or 3D representations of an item. For cards, only `.png` or `.jpg` are supported.

## Example JSON

```jsonc
{
  "version_str": "1.0",
  
  "metadata": {
    "game_title_str": "Your Game Name",
    "description_str": "A short summary of what the game is about.",
    "rules_url": "https://domain.com/game-rules.html",
    "author_str": "Game Author",
    "publisher_str": "Game Publisher",
    "release_year_int": 2024,
    "language_str": "en",
    "tags_list": ["card game", "family-friendly", "strategy"],
    "additional_notes_str": "Any other custom data or key-value info you'd like to include."
  },

  "decks_list": [
    {
      "deck_name_str": "Main Deck",
      "deck_card_back_url_png": "https://domain.com/path-to-card-back.png",
      "cards_list": [
        {
          "card_id_str": "CARD_UNIQUE_ID",
          "card_name_str": "Sample Card",
          "card_image_url_png": "https://domain.com/path-to-card-front.png",
          "card_description_str": "Optional short text about what this card does or means."
        }
      ]
    }
  ],

  "table_items_list": [
    {
      "item_name_str": "Score Pad",
      "item_png": "https://domain.com/score-pad.png",
      "item_usdz": "https://domain.com/score-pad.usdz"
    },
    {
      "item_name_str": "Counter Token",
      "item_png": "https://domain.com/token.png",
      "item_usdz": "https://domain.com/token.usdz"
    }
  ]
}
```

---

## Field-by-Field Explanation

### Top-Level Fields

- **`version_str`**  
  - **Type**: String  
  - **Purpose**: Denotes the version of this JSON format or data structure.  
  - **Required**: <span style="color:red">**Yes**</span>

### `metadata` Object

- **`metadata.game_title_str`**  
  - **Type**: String  
  - **Required**: <span style="color:red">**Yes**</span>
  - **Description**: The official title of your game.
- **`metadata.description_str`**  
  - **Type**: String  
  - **Optional**  
  - **Description**: Short summary or tagline about your game.
- **`metadata.rules_url`**  
  - **Type**: String (URL)
  - **Required**: <span style="color:red">**Yes**</span>
  - **Description**: URL pointing to an HTML page describing the rules.
- **`metadata.author_str`**  
  - **Type**: String  
  - **Optional**  
  - **Description**: Name of the primary designer or author of the game.
- **`metadata.publisher_str`**  
  - **Type**: String  
  - **Optional**  
  - **Description**: Name of the publisher or studio.
- **`metadata.release_year_int`**  
  - **Type**: Number  
  - **Optional**  
  - **Description**: Year of first release or publication.
- **`metadata.language_str`**  
  - **Type**: String  
  - **Optional**  
  - **Description**: Main language used, e.g. `"en"`.
- **`metadata.tags_list`**  
  - **Type**: Array of Strings  
  - **Optional**  
  - **Description**: Keywords or categories that apply to this game (e.g., `["card game", "family-friendly", "strategy"]`).
- **`metadata.additional_notes_str`**  
  - **Type**: String  
  - **Optional**  
  - **Description**: Miscellaneous notes or disclaimers.

### `decks_list` Array

- **Purpose**: Each deck describes a collection of cards. Must have at least one deck in the array.
- **`deck_name_str`**  
  - **Type**: String  
  - **Optional**  
  - **Description**: Label for this deck (e.g., `"Main Deck"` or `"Expansion Deck"`).
- **`deck_card_back_url_png`**  
  - **Type**: String (URL)  
  - **Optional**  
  - **Description**: URL to an image for the backside of all cards in this deck.  
  - You could also name it `_jpg` or `_usdz` if you prefer a different format, but typically a standard PNG or JPG is used for card backs.

#### `cards_list` Array

- **Purpose**: Holds all the card definitions for this deck.
- **`card_id_str`**  
  - **Type**: String  
  - **Required**: <span style="color:red">**Yes**</span> for each card to uniquely identify it.  
  - **Description**: A unique identifier or code (e.g., `"CARD_UNIQUE_ID"` or `"3+5"`).
- **`card_name_str`**  
  - **Type**: String  
  - **Optional**  
  - **Description**: The display name of the card (if different from the identifier).
- **`card_image_url_png`**  
  - **Type**: String (URL)  
  - **Required**: You need at least one form of image for the card.  
  - **Description**: URL to the card’s face image. Could be `_jpg` or `_usdz` as well.
- **`card_description_str`**  
  - **Type**: String  
  - **Optional**  
  - **Description**: A short note describing the card’s function, special effects, or flavor text.

> **Note**: Each deck must have **at least one card** in `cards_list`.

### `table_items_list` Array

- **Purpose**: Describes additional items that might be placed on the table or distributed to players (score pads, tokens, reference sheets, etc.).
- **`item_name_str`**  
  - **Type**: String  
  - **Optional** (the item is optional entirely)  
  - **Description**: Friendly name for the item (e.g., `"Score Pad"`).
- **`item_png`** / **`item_jpg`** / **`item_usdz`**  
  - **Type**: String (URL)  
  - **Optional**  
  - **Description**: Pointers to images or 3D objects. Only use **one** format per item. For example, if your item is a 3D model, use `item_usdz`; if it’s a flat reference, use `item_png` or `item_jpg`.

---

## Putting It All Together

1. **Create Your Metadata**  
   - At minimum, ensure `metadata.game_title_str` is present. Everything else in `metadata` is optional but recommended.

2. **Define Your Deck(s)**  
   - There must be at least one entry in `decks_list`.  
   - Each deck can have an optional back image link.  
   - Within each deck’s `cards_list`, you must have at least one card.

3. **(Optional) Table Items**  
   - Use `table_items_list` if your game involves extra components like tokens, score sheets, or reference sheets.

4. **Check Consistency**  
   - Make sure all URLs point to valid resources.  
   - Decide whether you want PNG, JPG, or 3D (USDZ) for each item or card image. Use only one per item or card.

5. **Save as JSON**  
   - Confirm the file is valid JSON (e.g., use a JSON validator).

---

### Minimal Valid Example

Below is the smallest possible JSON that meets the requirement of having a game title and at least one deck with one card:

```json
{
  "metadata": {
    "game_title_str": "Minimal Game"
  },
  "decks_list": [
    {
      "cards_list": [
        {
          "card_id_str": "CARD_1",
          "card_image_url_png": "https://domain.com/path-to-card-front.png"
        }
      ]
    }
  ]
}
```

This example has no optional fields. It simply includes:

- A title for the game.  
- One deck with one card (a unique card ID and an image link).

---

## Best Practices & Recommendations

- **Use Descriptive Identifiers**: For both deck names and card IDs, pick identifiers that make sense in your system (`"Main_Deck"`, `"1_plus_1"`, etc.).  
- **Keep URLs Accessible**: Ensure your media files are hosted or accessible wherever you plan to load this JSON.  
- **Versioning**: The `version_str` is based on the version of this document, which is currently <span style="color:red">**1.0**</span>.  
- **Validate Regularly**: Use a JSON validator to ensure your final file is valid JSON before distributing it.

With this structure and set of guidelines, you can scale your project from a simple single-deck game to a multi-deck, multi-expansion card system.