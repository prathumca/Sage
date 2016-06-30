# Fischer

Fischer is a cross-platform chess library for Swift.

## Features

### Game Management

Running a chess game can be as simple as setting up a loop.

```swift
import Fischer

let game = Game()

while !game.isFinished {
    let move = ...
    try game.execute(move: move)
}
```

### Move Generation

Fischer is capable of generating legal moves for the current player.

It fully supports special moves such as en passant and castling.

- `availableMoves()` will return all moves currently available.

- `movesForPiece(at:)` will return all moves for a piece at a square.

- `movesBitboardForPiece(at:)` will return a `Bitboard` containing all of the
  squares a piece at a square can move to.

### Move Validation

Fischer can also validate whether a move is legal with the `isLegal(move:)`
method for a `Game` state.

The `execute(move:)` family of methods calls this method, so it would be faster
to execute the move directly and catch any error from an illegal move.

### Undo and Redo Moves

Move undo and redo operations are done with the `undoMove()` and `redoMove()`
methods. The undone or redone move is returned.

To just check what moves are to be undone or redone, the `moveToUndo()` and
`moveToRedo()` methods are available.

### Promotion Handling

The `execute(move:promotion:)` method takes a closure that returns a promotion
piece. This allows for the app to prompt the user for a promotion piece or
perform any other operations before choosing a promotion piece.

```swift
try game.execute(move: move) {
    ...
    return .Queen(game.playerTurn)
}
```

The closure is only executed if the move is a pawn promotion. An error is thrown
if the promotion piece is the wrong color or cannot promote a pawn, such as with
a king or pawn.

A piece can be given without a closure. The default promotion piece is a queen.

```swift
try game.execute(move: move, promotion: .Queen(game.playerTurn))
```

### Pretty Printing

The `Board` and `Bitboard` types both have an `ascii` property that can be used
to print a visual board.

```swift
let board = Board()

print(board.ascii)
//   +-----------------+
// 8 | r n b q k b n r |
// 7 | p p p p p p p p |
// 6 | . . . . . . . . |
// 5 | . . . . . . . . |
// 4 | . . . . . . . . |
// 3 | . . . . . . . . |
// 2 | P P P P P P P P |
// 1 | R N B Q K B N R |
//   +-----------------+
//     a b c d e f g h

print(board.bitboard().ascii)
//   +-----------------+
// 8 | 1 1 1 1 1 1 1 1 |
// 7 | 1 1 1 1 1 1 1 1 |
// 6 | . . . . . . . . |
// 5 | . . . . . . . . |
// 4 | . . . . . . . . |
// 3 | . . . . . . . . |
// 2 | 1 1 1 1 1 1 1 1 |
// 1 | 1 1 1 1 1 1 1 1 |
//   +-----------------+
//     a b c d e f g h
```

## License

Fischer is published under version 2.0 of the Apache License.
