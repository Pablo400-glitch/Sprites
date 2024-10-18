# Sprites

En está práctica se añaden sprites a un proyecto de Unity y estas sprites tienen que estar animados.

## Actividad 1

En está primera actividad añadí al proyecto el personaje que aparece en la Figura 1. Tras añadirlo tuve que configurarlo como múltiple y subdivir el atlas de sprites en sprites independientes.

![alt text](images/1.png)

*Figura 1: Subdivisión del atlas de sprites del character*

## Actividad 2

![alt text](images/2.gif)

*Figura 2: Ejecución de la animación "Walk Down"*

## Actividad 3

```cs
public class PlayerMovement : MonoBehaviour
{
    [SerializeField]
    private float speed;

    private Rigidbody2D body;

    void Start()
    {
        body = GetComponent<Rigidbody2D>();
    }

    void FixedUpdate()
    {
        Movement();
    }

    private void Movement()
    {
        float x = Input.GetAxis("Horizontal");
        body.position += new Vector2(x, 0) * Time.deltaTime * speed;
    }
}
```

![alt text](images/3.png)

*Figura 3: Añadir controles por movimiento al gameObject*

![alt text](images/3.gif)

*Figura 4: Movimiento del jugador sin animación*

## Actividad 4

```cs
public class PlayerMovement : MonoBehaviour
{
    [SerializeField]
    private float speed;

    private Rigidbody2D body;
    private SpriteRenderer spriteRenderer;

    void Start()
    {
        body = GetComponent<Rigidbody2D>();
        spriteRenderer
         = GetComponent<SpriteRenderer>();
    }

    // Update is called once per frame
    void FixedUpdate()
    {
        Movement();
    }

    private void Movement()
    {
        float x = Input.GetAxis("Horizontal");

        if (x > 0)
        {
            spriteRenderer.flipX = true;
        }
        else if (x < 0)
        {

            spriteRenderer.flipX = false;
        }

        body.position += new Vector2(x, 0) * Time.deltaTime * speed;
    }
}
```

## Actividad 5

```cs
public class PlayerMovement : MonoBehaviour
{
    [SerializeField]
    private float speed;

    [SerializeField]
    private float distanceToChangeAnimation = 10.0f;

    private Rigidbody2D body;
    private SpriteRenderer spriteRenderer;
    private Animator animator;

    private Vector2 previousPosition;
    private float distanceTraveled = 0.0f;


    void Start()
    {
        body = GetComponent<Rigidbody2D>();
        spriteRenderer = GetComponent<SpriteRenderer>();
        animator = GetComponent<Animator>();

        previousPosition = transform.position;
    }

    void FixedUpdate()
    {
        Movement();
        CalculateDistanceTraveled();
    }

    private void Movement()
    {
        float x = Input.GetAxis("Horizontal");
        if (x < 0 || x > 0)
            animator.SetBool("Walk", true);
        else
        {
            animator.SetBool("Walk", false);
        }

        if (x > 0)
        {
            spriteRenderer.flipX = true;
        }
        else if (x < 0)
        {
            spriteRenderer.flipX = false;
        }

        body.position += new Vector2(x, 0) * Time.deltaTime * speed;
    }

    private void CalculateDistanceTraveled()
    {
        float distance = Vector2.Distance(transform.position, previousPosition);
        distanceTraveled += distance;
        previousPosition = transform.position;

        if (distanceTraveled >= distanceToChangeAnimation)
        {
            animator.SetBool("Walk", false);
            animator.SetBool("Dance", true);

            distanceTraveled = 0.0f;
        }
    }
}
```

![alt text](images/5.gif)

*Figura 5: Animación de baile tras una distancia caminando*

![alt text](images/5.png)

*Figura 6: Nodos del animator*