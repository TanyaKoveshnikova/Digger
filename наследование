# Digger
using System;
using System.Windows.Forms;

namespace Digger
{
    public class Terrain : ICreature
    {
        public string GetImageFileName() => "Terrain.png";
       
        public int GetDrawingPriority() => 2;
       
        public CreatureCommand Act(int x, int y) => new CreatureCommand();

        public bool DeadInConflict(ICreature conflictedObject) => true;
    }

    public class Player : ICreature
    {
        public string GetImageFileName() => "Digger.png";

        public int GetDrawingPriority() => 1;
        
        public CreatureCommand Act(int x, int y)
        {
            var moveCommand = new CreatureCommand();
            var key = Game.KeyPressed;
            if (key == Keys.Up || key == Keys.W)
                if (y >= 1)
                    moveCommand.DeltaY = -1;

            if (key == Keys.Down || key == Keys.S)
                if (y < Game.MapHeight - 1)
                    moveCommand.DeltaY = 1;

            if (key == Keys.Left || key == Keys.A)
                if (x >= 1)
                    moveCommand.DeltaX = -1;

            if (key == Keys.Right || key == Keys.D)
                if (x < Game.MapWidth - 1)
                    moveCommand.DeltaX = 1;
            if (Game.Map[x + moveCommand.DeltaX, y + moveCommand.DeltaY] is Sack)
                return new CreatureCommand {DeltaX = 0, DeltaY = 0};
            return moveCommand;
        }

        public bool DeadInConflict(ICreature conflictedObject)
        {
            return conflictedObject is Sack || conflictedObject is Monster;
        }
    }

    public class Sack : ICreature
    {
        private int countFall;

        public string GetImageFileName() => "Sack.png";

        public int GetDrawingPriority() => 0;

        public CreatureCommand Act(int x, int y)
        {
            var moveCommand = new CreatureCommand();
            if (y != Game.MapHeight - 1)
                if (Game.Map[x, y + 1] == null ||
                    (Game.Map[x, y + 1] is Player || Game.Map[x, y + 1] is Monster) && countFall > 0)
                {
                    countFall++;
                    moveCommand.DeltaY = 1;
                    return moveCommand;
                }

            if (countFall > 1)
            {
                moveCommand.TransformTo = new Gold();
                countFall = 0;
                moveCommand.DeltaY = 0;
                return moveCommand;
            }

            countFall = 0;
            moveCommand.DeltaY = 0;
            return moveCommand;
        }

        public bool DeadInConflict(ICreature conflictedObject) => false;
    }

    public class Gold : ICreature
    {
        public string GetImageFileName() => "Gold.png";
        
        public int GetDrawingPriority() => 3;

        public CreatureCommand Act(int x, int y) => new CreatureCommand();

        public bool DeadInConflict(ICreature conflictedObject)
        {
            if (conflictedObject is Monster)
                return true;
            Game.Scores += 10;
            return true;
        }
    }

    public class Monster : ICreature
    {
        public static bool PlayerLife;

        public string GetImageFileName() => "Monster.png";
       
        public int GetDrawingPriority() => 1;

        public CreatureCommand Act(int x, int y)
        {
            var playerCoordinates = FindPlayer();
            var create = new CreatureCommand();
            if (PlayerLife)
            {
                if (playerCoordinates[0] == x)
                    create.DeltaY = Math.Sign(playerCoordinates[1] - y);
                else if (playerCoordinates[1] == y)
                    create.DeltaX = Math.Sign(playerCoordinates[0] - x);
                else
                    create.DeltaX = Math.Sign(playerCoordinates[0] - x);
            }
            if (!(x + create.DeltaX >= 0 && x + create.DeltaX < Game.MapWidth 
			      && y + create.DeltaY >= 0 && y + create.DeltaY < Game.MapHeight))
                return Stay();
            var map = Game.Map[x + create.DeltaX, y + create.DeltaY];
            if (map is Terrain || map is Sack || map is Monster)
                return Stay();
            return new CreatureCommand {DeltaX = create.DeltaX, DeltaY = create.DeltaY};
        }

        public bool DeadInConflict(ICreature conflictedObject)
        {
            return conflictedObject is Monster || conflictedObject is Sack;
        }

        private static CreatureCommand Stay() => new CreatureCommand();

        private static int[] FindPlayer()
        {
            for (var x = 0; x < Game.MapWidth; x++)
            for (var y = 0; y < Game.MapHeight; y++)
                if (Game.Map[x, y] is Player)
                {
                    PlayerLife = true;
                    return new[] {x, y};
                }
				
            return new[] {0, 0};
        }
    }
}
