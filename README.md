# /**
 * This material is based upon work supported by 
 * the National Science Foundation under Grant No. 1140753.
 */

package sudoku.util.board;

import java.util.Stack;


/**
 * A sudoku board whose regions are boxes each with identical width and height
 */
public class RegularSudokuBoard implements SudokuBoard
{	
	private final int boardSize;
	private final int[] boardCells;
	private final Stack<Move> history;
	private int boxWidth;

	/**
	 * @param width - the width of a region
	 * @param height - the height of a region
	 */
	public RegularSudokuBoard(int width, int height)
	{
		boardSize = width * height;
		boardCells = new int[boardSize * boardSize];
		history = new Stack<Move>();
		boxWidth = width;
	}
	
	public void plug(){
		int number = 1;
		for (int i=0; i<boardCells.length; i++){
			if (isEmpty(i)){
				//setCell (getFirstDigit(number), i);
				setCell ((Integer.parseInt(Character.toString(String.valueOf(num).charAt(i))), i);
				number = number+10;
		}
		if (emptyCells() == 0){
			if (!isSolved()){
				unplug();
				number = number + 1;
	}}}}
	
	public int emptyCells(){
		int numEmpty = 0;
		for (int i=0; i<boardCells.length; i++){
			if (isEmpty(i)){
				numEmpty = numEmpty+1;
			}
			
	}
		return numEmpty;
	}
				
	public void unplug(){
		for (int i=0; i<history.size(); i++){
			unmove();
		}
	}
	public static int getFirstDigit(int num){
		return Integer.parseInt(Character.toString(String.valueOf(num).charAt(0)));
	}
	
	public void move(int num, int cell)
	{
		Move mints = new Move(num, cell);
		history.push(mints);
		setCell(num, cell);
	}
	public boolean isEmpty(int cell){
		if (valueAt(cell) == 0)
		return true;
		else return false;
	}
	
	public void move(Move move)
	{
		history.push(move);
		setCell(move.getNum(), move.getCell());
	}
	
	public void unmove()
	{
		if (history.isEmpty()){ 
			System.err.println("You can't unmove a move that you have not yet made");}
		resetCell(history.peek().getCell());
		history.pop();
	}
	
	public void setCell(int num, int cell)
	{
		boardCells[cell] = num;
	}
	
	public void resetCell(int cell)
	{
		setCell(0 , cell);
	}
	
	public boolean isLegal(int num, int cell)
	{
		int rowStart = getRow(cell)* boardSize;
		int colStart = getCol(cell);
		int boxX = (getCol(cell) / boxWidth);
		int boxY = (getRow(cell) / boxWidth);
		int newVal = 0;
		int boxStart = (boxX*boxWidth) + ((boxY*boxWidth) * boardSize);
	//	int boxEnd = (boxStart + (boxWidth-1)) + (boardSize * (boxWidth-1)); 
		if (num == 0) return false;
		for (int i=rowStart; i<(rowStart+boardSize); i++){
					newVal = valueAt(i);
					if ((num == newVal) && (i != cell)) return false;
					}
		for (int i=colStart; i<(boardCells.length); i+=boardSize){
					newVal = valueAt(i);
					if ((num == newVal) && (i != cell)) return false;
					}	
		for (int i= boxStart; i<(boxStart + boxWidth); i++){
			for (int j=0; j<(boxWidth); j++){
						newVal = valueAt(i+(j * boardSize)); 
				if ((num == newVal) && (i+(j * boardSize) != cell)) return false;
			}}
		return true; }
		
	
	public boolean isLegal(Move move)
	{
		return isLegal(move.getNum(), move.getCell());
	}
	
	public Stack<Move> history()
	{
		
		return history;
	}
	
	public int size()
	{
		return boardSize;
	}
	
	public boolean isSolved()
	{
		for (int i=0; i<boardCells.length; i++){
			if (! isLegal(valueAt(i),i) || (valueAt(i)==0)) return false; 
		}
		
			System.out.println("By God, You've Done It!");
			return true;	
	}
	
	
	public int valueAt(int cell)
	{
		return boardCells[cell];
	}
	
	
	
	public int getRow(int cell)
	{
		return (cell / boardSize);
	}
	
	public int getCol(int cell)
	{
		return (cell % boardSize);
	}
	
	public int getBox(int cell)
	{
		int row = getRow(cell);
		int col = getCol(cell);
		int box = (boxWidth);
		
		int boxRow = (row / box);
		int boxCol = (col / box);
		
		return boxCol + boxRow * box;
		
	}
	
	@Override
	public String toString()
	{
		StringBuilder builder = new StringBuilder("");
		builder.append("Board size: "+boardSize+ " X " + boardSize +"\n\n");
		for(int i = 0; i < Math.pow(boardSize, 2); i++)
		{
			// builder.append(boardCells[i] == 0 ? "_" : Integer.toString(boardCells[i], boardSize + 1));
			builder.append(boardCells[i] == 0 ? String.format("%3s", "_") : String.format("%3s", ""+boardCells[i]));
			builder.append(" ");
			if(i % boardSize == boardSize - 1)
				builder.append("\n");
		}
		return builder.toString();
	}
}
