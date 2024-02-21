## Pooling 

Pooling is a common pattern in video-games in order to increase performance when instantiating many objects.
This pooling library takes hints from Unity's first (and last) open project, of which you can read their wiki 
article on pooling [here](https://github.com/UnityTechnologies/open-project-1/wiki/Object-pooling). 

### [Factory](Runtime/Factory.cs)
Creates a factory for a component called `MyComponent`
```csharp
public class MyComponentFactory : Factory<MyComponent>
{
    public override MyComponent CreateInstance() {
        return new MyComponent();
    }
}
```

### [Component Pool](Runtime/ComponentPool.cs)
Create a pool for the `MyComponentFactory` above.
```csharp
[CreateAssetMenu(fileName = "MyPool", menuName = "Pool/MyPool")]
public class MyPool : ComponentPool<MyComponent>
{
	[field: SerializeField] private MyComponentFactory factory { get; set; }
	[field: SerializeField] private int initialPoolSize { get; set; }
}
```

Example usage during runtime
```csharp
public class RuntimeScript : MonoBehaviour 
{
    [SerializeField] private MyPool pool;
    
    private void Awake()
    {
        pool = ScriptableObject.CreateInstance<MyPool>(); 
        pool.Factory = ScriptableObject.CreateInstance<MyComponentFactory>(); 
    }
    
    private void Start() 
    {
        pool.Prewarm(10);
        MyComponent component = pool.Request(); 
        // do things with the component then
        pool.Return(component); 
    }
}
```
