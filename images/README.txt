1
Añadir Imagen

2
Añadir Gif
Añadir Imagen

3
Añadir Script
public class PlayerMovement : MonoBehaviour
{
    [SerializeField]
    private float speed;

    private Rigidbody2D body;

    void Start()
    {
        body = GetComponent<Rigidbody2D>();
    }

    // Update is called once per frame
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

4
Añadir Script
public class PlayerMovement : MonoBehaviour
{
    [SerializeField]
    private float speed;

    private Rigidbody2D body;
    private SpriteRenderer spriteRenderer;

    void Start()
    {
        body = GetComponent<Rigidbody2D>();
        spriteRenderer = GetComponent<SpriteRenderer>();
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
Añadir Gif

5
Añadir Script
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

Añadir Gif

