### Class HexGrid
#### DestroyPiecesAndCountVPs()
- In: Ref INT player1VPs, Ref INT player2VPs
- Out: bool
```
baronDestroyed = false
listOfTilesContainingDestroyedPieces = NEW List<Tile> 
FOREACH var t in tiles
    // See if the tiles have a piece
    IF t.GetPieceInTile() NOT null
        listOfNeighbours = NEW List<Tile>(t.GetNeighbours())
        noOfConnections = 0
        // Looks for the number of pieces around the current piece
        FOREACH var n in listOfNeigbours
            IF n.GetPieceInTile() NOT null
                noOfConnections += 1
            ENDIF
        ENDFOREACH
        Piece thePiece = t.GetPieceInTile()
        // See if the piece needs to be destroyed
        IF noOfConnections >= thePiece.GetConnectionsNeededToDestroy()
            thePiece.DestroyPiece()
            // the piece property: destroyed is true
            // but this property is not used in the program
            IF thePiece.GetPieceType().ToUpper() = "B"
                baronDestroyed = True
                listOfTilesContainingDestroyedPieces.Add(t)
                // Add the baron to the list which contains destroyed pieces
            ENDIF
            IF thePiece.GetBelongsToPlayer1()
                player2VPs += thePiece.GetVPs()
                // VPs to player 2 if piece belongs to player 1
            ELSE 
                player1VPs += thePiece.GetVPs()
                // VPs to player 1 if piece belongs to player 2
            ENDIF
        ENDIF
    ENDIF
ENDFOREACH
// after finishing checking all the tiles, clear the tiles that needs to be destroyed
FOREACH var t in listOfTilesContainingDestroyed 
    t.SetPiece(null)
ENDFOREACH
RETURN baronDestroyed
```