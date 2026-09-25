# Apprentissage_Unity
Pour stage L2

# code Player_Attack

using UnityEditor;
using UnityEngine;
using UnityEngine.InputSystem;
using UnityEngine.Rendering;



public class PlayerAttack : MonoBehaviour
{
    public float timeBtwAttack;
    public float TimeAttack; 
    // public Transform attackPos;
    // public Transform attackPos2;
    public LayerMask isEnemy;
    public float attackRange;
    public Vector2 attackRange2;

    public Transform Attack1;
    public Transform Attack2;
    private Player P;

    public SpriteRenderer sr;

    void Start()
    {
        P = GetComponent<Player>();
        sr = GetComponent<SpriteRenderer>();

       
    }

    void FixedUpdate()
    {

        if (sr.flipX == true)
        {
            Attack1.localPosition = new Vector2(-1.74f, Attack1.localPosition.y);
            Attack2.localPosition = new Vector2(-1.83f, Attack2.localPosition.y);
        }
        else
        {
            Attack1.localPosition = new Vector2(1.74f, Attack1.localPosition.y);
            Attack2.localPosition = new Vector2(1.83f, Attack2.localPosition.y);    
        }
        
        if(Time.time > timeBtwAttack)
        {
            if (Keyboard.current.enterKey.wasPressedThisFrame && P.IsGrounded() && P.canMoving)
            {
                P.rb.linearVelocity = Vector2.zero;
                P.anim.SetTrigger("Attack1");
                timeBtwAttack = Time.time + TimeAttack;
                Attack(3);
            }
        }
        if (Time.time > timeBtwAttack)
        {
            if (Keyboard.current.shiftKey.wasPressedThisFrame && P.IsGrounded() && P.canMoving)
            {
                P.rb.linearVelocity = Vector2.zero;
                P.anim.SetTrigger("Attack2");
                timeBtwAttack = Time.time + TimeAttack;
                Scnd_Attack(1);
            }
        }
    }

    void OnDrawGizmos()
    {
        if (Attack1 != null)
        {
            Gizmos.DrawWireSphere(Attack1.position, attackRange);
        }
        if (Attack2 != null)
        {
            Gizmos.DrawWireCube(Attack2.position, attackRange2);
        }
    }

    public void canMove()
    {
        P.canMoving = !P.canMoving;
    }

    public void Attack(int damage)
    {
        if(Physics2D.OverlapCircle(Attack1.position, attackRange, isEnemy))
        {
            Collider2D enemiesToDamage = Physics2D.OverlapCircle(Attack1.position, attackRange, isEnemy);
            enemiesToDamage.GetComponent<Enemy>().TakeDamage(damage);
      
        }else
        {
            Debug.Log("No enemies in range");
        }
    }

  
    public void Scnd_Attack(int damage)
    {
        if(Physics2D.OverlapBox(Attack2.position, attackRange2, 0f, isEnemy))
        {
            Collider2D enemiesToDamage = Physics2D.OverlapBox(Attack2.position, attackRange2, 0f, isEnemy);
                enemiesToDamage.GetComponent<Enemy>().TakeDamage(damage);
        }else
        {
            Debug.Log("No enemies in range");
        }
    }
}

