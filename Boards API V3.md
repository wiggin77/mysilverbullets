# Boards API V3

## Boards

### createBoard
POST /boards

Creates a new board. 

Body: model.Board

Returns: model.Board

### getBoards
GET /teams/{teamID}/boards

Gets all boards for the specified team. 

Returns: []model.Board

**Changes:** query should be bounded via pagination. (default=100, max=200)


### getBoard
GET /boards/{boardID}

Gets the specified board.

Returns: model.Board

### patchBoard
PATCH /boards/{boardID}

Patches the specified board.

Body: model.BoardPatch

Returns: model.Board

### deleteBoard
DELETE /boards/{boardID}

Deletes the specified board and children (cards, blocks, etc).

**Changes:** clean up orphans - this is currently a hard delete that orphans all children (cards, blocks, etc).

### duplicateBoard
POST /boards/{boardID}/duplicate

Duplicates the specified board and all children (cards, blocks, etc).

Returns: model.BoardsAndBlocks

**Changes:** return model.Board; client can fetch cards via getCards which supports pagination. 

### undeleteBoard
POST /boards/{boardID}/undelete

Undeletes a board and all children.

**Changes:** return model.Board; undelete children

### getBoardMetadata
GET /boards/{boardID}/metadata

Gets metadata for the specified board. 

Returns: model.BoardMetadata

## Cards

### createCard
POST /boards/{boardID}

Creates a new card.

Body: model.Card
Query Params: 
  disable_notify: true if notifications should be disabled (def=false)

Returns: model.Card

### getCard
GET /cards/{cardID}

Gets the specified card.

Returns: model.Card

### getCards
GET /boards/{boardID}

Gets cards for the specified board.

Query Params: 
  page: (int) The page to select (default=0)
  per_page: (int) The number of cards to return. (default=100, max=200)

Returns: []model.Card

### patchCard
PATCH /cards/{cardID}

Patches the specified card and/or card properties.

Body: model.CardPatch

Returns: model.Card

### deleteCard
DELETE /cards/{cardID}

Deletes the specified card and child blocks.

### undeleteCard
POST /cards/{cardID}

Undeletes the specified card and all children.

Returns: model.Card

### copyCard
POST /cards/{cardID}/copy

Copies a card to a board. If the destination board is not the board the card belongs to, best efforts are made to preserve card properties that match property definitions for the destination board. If `move` is true then the original card is deleted after the copy. 

Body: model.CopyCard
{
  boardID: {destinationBoardID},
  move: {boolean}
}

Returns: model.Card - the new card

## Comments

### CreateComment
POST /cards/{cardID}/comments

Body: model.Comment
