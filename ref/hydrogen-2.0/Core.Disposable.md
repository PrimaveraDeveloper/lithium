# Disposable

## FAQ

**What is the `IDisposable` interface?**  

- Provides a mechanism for releasing unmanaged resources. The primary use of this interface is to **release unmanaged resources**. 

- The garbage collector automatically releases the memory allocated to a managed object when that object is no longer used. However, it is not possible to predict when garbage collection will occur. 

- Furthermore, the garbage collector has no knowledge of unmanaged resources such as window handles, or open files and streams.

- Use the Dispose method of this interface to explicitly release unmanaged resources in conjunction with the garbage collector. The consumer of an object can call this method when the object is no longer needed.

**When to implement the `IDisposable`?**  

- If my class handles with unmanaged resources (file, network, database, legacy objects, operative system objects).

**Which objects should be disposed of?**

- First, dispose of objects in the `Dispose(bool)` method of your class, do not dispose in the `IDisposable.Dispose()` method.

- Dispose of all objects for which the **owner is my class**
  - created objects `T obj = new T()`
  - transient objects of dependency injection

**Which objects should not be disposed of?**

- Do not dispose of any object that your class is not the owner.

- Do not dispose of objects that you receive as input parameter (ctor, method) and save an internal reference, because that does not mean your class is the owner.

**How to implement the `IDisposable` interface?**

- Follow the examples presented below.

- Follow the official links:
  - https://docs.microsoft.com/en-us/dotnet/api/system.idisposable?view=netstandard-2.1
  - https://docs.microsoft.com/en-us/dotnet/api/system.idisposable?view=netcore-3.1
  - https://docs.microsoft.com/en-us/dotnet/api/system.idisposable?view=netframework-4.8

## Examples

**Example 1**  

A disposable base class

```csharp
    /// <summary>
    /// Provides a base class for creating a class that needs to implement the IDisposable pattern.
    /// </summary>
    public abstract class DisposableBase : IDisposable
    {
        #region Constructors

        /// <summary>
        /// Initializes a new instance of the <see cref="DisposableBase"/> class.
        /// </summary>
        protected DisposableBase()
            : base()
        {
            // Initialize disposed state            
            this.Disposed = false;
        }

        #endregion

        #region Destructor

        /// <summary>
        /// Finalizes an instance of the <see cref="DisposableBase"/> class.
        /// </summary>
        /// <remarks>
        /// Releases unmanaged resources and performs other cleanup operations before the
        /// <see cref="DisposableBase"/> is reclaimed by garbage collection.
        /// This destructor will run only if the Dispose method does not get called.
        /// It gives your base class the opportunity to finalize.
        /// Leave out the <c>finalizer</c> altogether if this class doesn't own unmanaged resources itself,
        /// but leave the other methods exactly as they are.
        /// </remarks>
        ~DisposableBase()
        {
            // Override the finalizer only if Dispose(bool disposing) below has any code to free unmanaged resources.
            // Do not change this code, any cleanup code should go to the Dispose(bool disposing).

            this.Dispose(false);
        }

        #endregion

        #region IDisposable Members

        /// <summary>
        /// Gets or sets a value indicating whether this instance is disposed.
        /// </summary>
        protected bool Disposed { get; set; }

        /// <summary>
        /// Called whenever the object instance needs to clean up the memory used by this component.
        /// A derived class should not be able to override this method.
        /// </summary>
        public void Dispose()
        {
            // Do not change this code, any cleanup code should go to the Dispose(bool disposing).

            this.Dispose(true);

            // Uncomment the following line if the finalizer is overridden above.

            // This object will be cleaned up by the Dispose method.
            // Therefore, you should call GC.SupressFinalize to take this object off the finalization queue
            // and prevent finalization code for this object from executing a second time.

            GC.SuppressFinalize(this);
        }

        /// <summary>
        /// Releases unmanaged and (optionally) managed resources.
        /// Called whenever the object instance needs to clean up.
        /// <c>Dispose(bool disposing)</c> executes in two distinct scenarios:
        /// If disposing equals true, the method has been called directly or indirectly by user's code,
        /// therefore both managed and unmanaged resources can be disposed.
        /// If disposing equals false, the method has been called by the runtime from inside the <c>finalizer</c> and you should not reference other objects,
        /// therefore only unmanaged resources can be disposed.
        /// </summary>
        /// <param name="disposing"><c>true</c> to release both managed and unmanaged resources; <c>false</c> to release only unmanaged resources.</param>
        protected virtual void Dispose(bool disposing)
        {
            // If not disposed, to avoid redundant calls

            if (!this.Disposed)
            {
                // Dispose managed state

                if (disposing)
                {
                    // Insert your code here:
                    // - Dispose of managed objects
                    // - Set null references of managed objects
                    // - Unsubscribe of all managed objects events
                }

                // Free unmanaged resources

                // Insert your code here (and override the finalizer):
                // - Free your own state of unmanaged objects
                // - Release pointers to unmanaged window handles
                // - Set null references of unmanaged objects
                // - Set null of large fields
                // - Set null of delegates and events exposed to COM

                // Finally, update the disposing state

                this.Disposed = true;

                // Optionally, if you want to free any memory immediately

                // Force GC
                // GC.Collect();
                // GC.WaitForPendingFinalizers();
                // GC.Collect();
            }
        }

        #endregion
    }
```

**Example 2**

A disposable class that inherits from `DisposableBase`.

```csharp
namespace Primavera.Taskbox.Core
{
    /// <summary>
    /// Example of a disposable class that inherits from the <see cref="DisposableBase"/>.
    /// </summary>
    /// <remarks>
    /// The derived class does not have a Finalize method (~destructors cannot be called, they are invoked automatically)
    /// or a Dispose method with parameters because it inherits them from the base class.
    /// </remarks>
    public class DisposableSample1 : DisposableBase
    {
        #region Constructors

        /// <summary>
        /// Initializes a new instance of the <see cref="DisposableSample1"/> class.
        /// </summary>
        public DisposableSample1()
            : base() // IDisposable, initialize disposed state
        {
        }

        #endregion

        #region IDisposable Members (overrides)

        /// <summary>
        /// Releases unmanaged and (optionally) managed resources.
        /// Called whenever the object instance needs to clean up.
        /// Dispose(bool disposing) executes in two distinct scenarios:
        /// If disposing equals true, the method has been called directly or indirectly by user's code,
        /// therefore both managed and unmanaged resources can be disposed.
        /// If disposing equals false, the method has been called by the runtime from inside the <c>finalizer</c> and you should not reference other objects,
        /// therefore only unmanaged resources can be disposed.
        /// </summary>
        /// <param name="disposing"><c>true</c> to release both managed and unmanaged resources; <c>false</c> to release only unmanaged resources.</param>
        protected override void Dispose(bool disposing)
        {
            // If not disposed, to avoid redundant calls

            if (!this.Disposed)
            {
                // Dispose managed state

                if (disposing)
                {
                    // Insert your code here:
                    // - Dispose of managed objects
                    // - Set null references of managed objects
                    // - Unsubscribe of all managed objects events
                }

                // Free unmanaged resources

                // Insert your code here (and override the finalizer):
                // - Free your own state of unmanaged objects
                // - Release pointers to unmanaged window handles
                // - Set null references of unmanaged objects
                // - Set null of large fields
                // - Set null of delegates and events exposed to COM

                // Finally, update the disposing state

                this.Disposed = true;

                // Optionally, if you want to free any memory immediately

                // Force GC
                // GC.Collect();
                // GC.WaitForPendingFinalizers();
                // GC.Collect();
            }

            // Dispose on base class
            // CA2215: Ensure that method calls base.Dispose in all possible control flow paths.

            base.Dispose(disposing);
        }

        #endregion
    }
}
```

**Example 3**

A disposable class that cannot inherit from `DisposableBase`.

```csharp
using System;

namespace Primavera.Taskbox.Core
{
    /// <summary>
    /// Example of a disposable class that cannot inherit from the <see cref="DisposableBase"/>.
    /// </summary>
    /// <remarks>
    /// The classes that cannot inherit from <see cref="DisposableBase"/>, 
    /// because multiple inheritance is not supported in C#,
    /// should implement the full pattern explicitly.
    /// </remarks>
    public class DisposableSample2 : IDisposable
    {
        #region Constructors

        /// <summary>
        /// Initializes a new instance of the <see cref="DisposableSample2"/> class.
        /// </summary>
        public DisposableSample2()
        {
            // Initialize disposed state

            this.Disposed = false;
        }

        #endregion

        #region Destructors

        /// <summary>
        /// Finalizes an instance of the <see cref="DisposableSample2"/> class.
        /// </summary>
        /// <remarks>
        /// Releases unmanaged resources and performs other cleanup operations before the
        /// <see cref="DisposableSample1"/> is reclaimed by garbage collection.
        /// This destructor will run only if the Dispose method does not get called to
        /// give your class the opportunity to finalize.
        /// Leave out the <c>finalizer</c> altogether if this class doesn't own unmanaged resources itself,
        /// but leave the other methods exactly as they are.
        /// </remarks>
        ~DisposableSample2()
        {
            // Override the finalizer only if Dispose(bool disposing) below has any code to free unmanaged resources.
            // Do not change this code, any cleanup code should go to the Dispose(bool disposing).

            this.Dispose(false);
        }

        #endregion

        #region Private Properties

        /// <summary>
        /// Gets or sets a value indicating whether this instance has already been disposed.
        /// </summary>
        private bool Disposed { get; set; }

        #endregion

        #region IDisposable Members

        /// <summary>
        /// Releases unmanaged and (optionally) managed resources.
        /// Called whenever the object instance needs to clean up the memory used by this component.
        /// </summary>
        public void Dispose()
        {
            // Do not change this code, any cleanup code should go to the Dispose(bool disposing).

            this.Dispose(true);

            // Uncomment the following line if the finalizer is overridden.

            // This object will be cleaned up by the Dispose method.
            // Therefore, you should call GC.SupressFinalize to take this object off the finalization queue
            // and prevent finalization code for this object from executing a second time.

            GC.SuppressFinalize(this);
        }

        /// <summary>
        /// Releases unmanaged and (optionally) managed resources.
        /// Called whenever the this instance needs to clean up.
        /// <c>Dispose(bool disposing)</c> executes in two distinct scenarios:
        /// If disposing equals true, the method has been called directly or indirectly by user's code,
        /// therefore both managed and unmanaged resources can be disposed.
        /// If disposing equals false, the method has been called by the runtime from inside the <c>finalizer</c> and you should not reference other objects,
        /// therefore only unmanaged resources can be disposed.
        /// </summary>
        /// <param name="disposing"><c>true</c> to release both managed and unmanaged resources; <c>false</c> to release only unmanaged resources.</param>
        protected virtual void Dispose(bool disposing)
        {
            // If not disposed, to avoid redundant calls

            if (!this.Disposed)
            {
                // Dispose managed state

                if (disposing)
                {
                    // Insert your code here:
                    // - Dispose of managed objects
                    // - Set null references of managed objects
                    // - Unsubscribe of all managed objects events
                }

                // Free unmanaged resources

                // Insert your code here (and override the finalizer):
                // - Free your own state of unmanaged objects
                // - Release pointers to unmanaged window handles
                // - Set null references of unmanaged objects
                // - Set null of large fields
                // - Set null of delegates and events exposed to COM

                // Finally, update the disposing state

                this.Disposed = true;

                // Optionally, if you want to free any memory immediately

                // Force GC
                // GC.Collect();
                // GC.WaitForPendingFinalizers();
                // GC.Collect();
            }
        }

        #endregion
    }
}
```