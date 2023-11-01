import java.util.Random;

// Класс босса
class Boss {
    int health;
    int damage;
    boolean stunned;

    public Boss() {
        health = 500;
        damage = 50;
        stunned = false;
    }

    public void takeDamage(int damage) {
        health -= damage;
    }

    public void setStunned(boolean stunned) {
        this.stunned = stunned;
    }

    public boolean isStunned() {
        return stunned;
    }
}

// Класс Golem
class Golem extends Hero {
    public Golem() {
        super(200, 30, "ROCK SHIELD");
    }

    @Override
    public void applySuperAbility(Boss boss) {
        int damageTaken = boss.damage / 2;
        health -= damageTaken;
        int golemDamage = damage + damageTaken;
        boss.takeDamage(golemDamage);
        System.out.println("Golem применил суперспособность " + superAbilityType);
    }
}

// Класс Magic
class Magic extends Hero {
    private int attackIncrease;

    public Magic() {
        super(100, 40, "MAGIC BOOST");
        attackIncrease = 10;
    }

    @Override
    public void applySuperAbility(Boss boss) {
        attack += attackIncrease;
        System.out.println("Magic применил суперспособность " + superAbilityType);
    }
}

// Класс Warrior
class Warrior extends Hero {
    public Warrior() {
        super(150, 50, "CRITICAL STRIKE");
    }

    @Override
    public void applySuperAbility(Boss boss) {
        int criticalMultiplier = new Random().nextInt(3) + 2;
        int warriorDamage = damage * criticalMultiplier;
        boss.takeDamage(warriorDamage);
        System.out.println("Warrior применил суперспособность " + superAbilityType);
    }
}

// Класс Thor
class Thor extends Hero {
    public Thor() {
        super(120, 45, "LIGHTNING STRIKE");
    }

    @Override
    public void applySuperAbility(Boss boss) {
        if (!boss.isStunned()) {
            boss.setStunned(true);
            System.out.println("Thor применил суперспособность " + superAbilityType + " и оглушил босса.");
        } else {
            System.out.println("Thor применил суперспособность " + superAbilityType + ", но босс уже оглушен.");
        }
    }
}

// Класс Tank
class Tank extends Hero {
    public Tank() {
        super(300, 20, "IRON DEFENSE");
    }

    @Override
    public void applySuperAbility(Boss boss) {
        int tankDamage = boss.damage / 5;
        boss.takeDamage(tankDamage);
        System.out.println("Tank применил суперспособность " + superAbilityType);
    }
}

// Класс Witcher
class Witcher extends Hero {
    private boolean revived;

    public Witcher() {
        super(120, 0, "LIFE TRANSFUSION");
        revived = false;
    }

    @Override
    public void applySuperAbility(Boss boss) {
        if (!revived) {
            if (isAlive()) {
                for (Hero hero : heroes) {
                    if (!hero.isAlive()) {
                        hero.revive();
                        health = 0;
                        revived = true;
                        System.out.println("Witcher применил суперспособность " + superAbilityType + " и оживил героя.");
                        return;
                    }
                }
            }
        }
        System.out.println("Witcher применил суперспособность " + superAbilityType + ", но не смог оживить героя.");
    }
}
