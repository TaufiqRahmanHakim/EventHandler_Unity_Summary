**Penjelasan EventHandler dengan contoh**

# Event Handler Unity

## Kelas ContainerCounter

Kelas `ContainerCounter` bertanggung jawab untuk mendeteksi saat pemain berinteraksi dengan objek wadah. Kelas ini menggunakan event handler untuk memberi tahu kelas lain saat hal ini terjadi.

```csharp
public class ContainerCounter : BaseCounter
{

    public event EventHandler OnPlayerGrabbedObject;

    [SerializeField] private KitchenObjectSO kitchenObjectSO;

    public override void Interact(Player player)
    {
        if (!player.HasKitchenObject())
        {
            KitchenObject.SpawnKitchenObject(kitchenObjectSO, player);
            OnPlayerGrabbedObject?.Invoke(this, EventArgs.Empty);
        }
    }
}
```

## Setup Event Handler

Event handler didefinisikan menggunakan delegasi `EventHandler`. Ini memungkinkan kelas lain untuk berlangganan event dan menerima pemberitahuan saat event dipicu.

```csharp
public event EventHandler OnPlayerGrabbedObject;
```
```csharp
OnPlayerGrabbedObject?.Invoke(this, EventArgs.Empty);
```
```csharp
private void Start()
    {
        containerCounter.OnPlayerGrabbedObject += ContainerCounter_OnPlayerGrabbedObject;
    }
```
```csharp
private void ContainerCounter_OnPlayerGrabbedObject(object sender, System.EventArgs e)
    {
        animator.SetTrigger(OPEN_CLOSE);
    }
```

## Implementasi Event Handler

Event handler diimplementasikan dengan cara berlangganan event dan mendefinisikan fungsi balikan (callback) yang akan dijalankan saat event dipicu.

```csharp
public class CountainerCounterVisual : MonoBehaviour
{
    private const string OPEN_CLOSE = "OpenClose";

    [SerializeField] private ContainerCounter containerCounter;

    private Animator animator;

    private void Awake()
    {
        animator = GetComponent<Animator>();
    }

    private void Start()
    {
        containerCounter.OnPlayerGrabbedObject += ContainerCounter_OnPlayerGrabbedObject;
    }

    private void ContainerCounter_OnPlayerGrabbedObject(object sender, System.EventArgs e)
    {
        animator.SetTrigger(OPEN_CLOSE);
    }
}
```

Pada contoh ini, kelas `ContainerCounterVisual` berlangganan event `OnPlayerGrabbedObject` dan mendefinisikan fungsi balikan yang mengatur trigger `OPEN_CLOSE` pada komponen animator. Ini akan menyebabkan animator memainkan animasi untuk membuka wadah.

Berikut adalah penjelasan lebih lanjut tentang setiap bagian kode:

* **Kelas ContainerCounter**

Kelas `ContainerCounter` memiliki properti `public event EventHandler OnPlayerGrabbedObject;`. Properti ini mendefinisikan event handler yang akan digunakan untuk memberi tahu kelas lain saat pemain berinteraksi dengan wadah.

Metode `public override void Interact(Player player);` kelas `ContainerCounter` memeriksa apakah pemain memiliki objek dapur. Jika tidak, metode ini akan membuat objek dapur baru dan kemudian memanggil event handler `OnPlayerGrabbedObject`.

* **Pengaturan Event Handler**

Delegasi `EventHandler` adalah delegasi yang digunakan untuk mendefinisikan event handler. Delegasi ini memiliki dua parameter:

    * `sender`: Objek yang memicu event.
    * `e`: Objek yang berisi informasi tentang event.

Dalam contoh ini, delegasi `EventHandler` digunakan untuk mendefinisikan event handler `OnPlayerGrabbedObject`. Event handler ini memiliki dua parameter:

    * `sender`: Objek `ContainerCounter` yang memicu event.
    * `e`: Objek `EventArgs.Empty` yang tidak berisi informasi apa pun.

* **Implementasi Event Handler**

Event handler diimplementasikan dengan cara berlangganan event dan mendefinisikan fungsi balikan yang akan dijalankan saat event dipicu.

Dalam contoh ini, kelas `ContainerCounterVisual` berlangganan event `OnPlayerGrabbedObject` kelas `ContainerCounter`. Kelas `ContainerCounterVisual` kemudian mendefinisikan fungsi balikan `private void ContainerCounter_OnPlayerGrabbedObject(object sender, System.EventArgs e);`. Fungsi balikan ini mengatur trigger `OPEN_CLOSE` pada komponen animator.

Semoga ini membantu!
     
