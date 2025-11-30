первый класс EnemyStats

using System;
using System.Collections.Generic;

public class EnemyStats
{
    public float Health { get; set; }
    public float Speed { get; set; }
    public int Reward { get; set; }

    public EnemyStats(float health, float speed, int reward)
    {
        Health = health;
        Speed = speed;
        Reward = reward;
    }

    public override string ToString() => $"HP:{Health} SPD:{Speed} REW:{Reward}";


    public class AverageHealthComparer : IComparer<EnemyStats>
    {
        public int Compare(EnemyStats a, EnemyStats b)
        {
            if (a == null || b == null) throw new ArgumentNullException();

            int result = a.Health.CompareTo(b.Health);  // ascending
            if (result != 0) return result;

            return a.Speed.CompareTo(b.Speed); // если здоровье равно → сортировка по скорости
        }
    }
    public class SpeedComparer : IComparer<EnemyStats>
    {
        public int Compare(EnemyStats a, EnemyStats b)
        {
            if (a == null || b == null) throw new ArgumentNullException();

            int result = b.Speed.CompareTo(a.Speed); // descending
            if (result != 0) return result;

            return a.Reward.CompareTo(b.Reward); // равная скорость → награда ascending
        }
    }
}

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

второй класс TowerStats

using System;
using System.Collections;
using System.Collections.Generic;

public class TowerStats : IEnumerable<EnemyStats>
{
    List<EnemyStats> enemies = new();

    public void AddEnemy(EnemyStats e) => enemies.Add(e);

    public IEnumerator<EnemyStats> GetEnumerator() => new TowerEnumerator(enemies);
    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();

    class TowerEnumerator : IEnumerator<EnemyStats>
    {
        List<EnemyStats> list;
        int index = -1;

        public TowerEnumerator(List<EnemyStats> list) => this.list = list;

        public EnemyStats Current => list[index];
        object IEnumerator.Current => Current;
        
        public bool MoveNext()
        {
            if (index + 1 < list.Count)
            {
                index++;
                return true;
            }
            return false;
        }

        public void Reset() => index = -1;
        public void Dispose() { }
    }
}
