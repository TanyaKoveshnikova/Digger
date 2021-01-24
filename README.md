# Digger
using System.Windows.Forms;

namespace Digger
{
    public class Terrain : ICreature
    {
        public string GetImageFileName()
        {
            return "Terrain.png";
        }

        public int GetDrawingPriority()
        {
            return 1;
        }

        public CreatureCommand Act(int x, int y)
        {
            return new CreatureCommand ();
        }

        public bool DeadInConflict(ICreature conflictedObject)
        {
            return true;
        }
    }

    public class Player : ICreature
    {
        public string GetImageFileName()
        {
            return "Digger.png";
        }

        public int GetDrawingPriority()
        {
            return 0;
        }

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

            return moveCommand;
        }

        public bool DeadInConflict(ICreature conflictedObject)
        {
            return false;
        }
    }
}
