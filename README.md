# MutatR
Single .tt file to auto generate immutable class mutation methods

# How to use
1. Just copy `MutatR.tt` into project file, you want to modify, no matter at which location there (for example project's root). Then specify `CustomTool`option in file properties as `TextTemplatingFileGenerator`, which allow T4 processor to generate code follow this template. Anyway, at this moment impossible to generate code using outside of MS VisualStudio, due to some limitation of resolving project items with T4. 
2. Mark any classes, designed to be immutable with `MutatR.ImmutableAttribute` and make them `partial` keyword which enables lookup them by generator and extending their functionality outside.
3. Execute `Run Custom Tool` action on `MutatR.tt` (Right click on `MutatR.tt`) to regenarate mutation infrastructure for classes.
4. Use it! Just call `Mutate()` on class instance to begin mutation process, then you can to use generated `With*(value)` methods in fluent manner to change values of related fields and properties, after that call `Commit()` to receive updated copy of your object.

# Example
```
namespace MutatR.Demo
{
    [MutatR.Immutable]
    internal partial class ImmutableTest
    {
        #region Fields

        public readonly double DoubleField;
        public readonly int IntField;
        public readonly string StringField;

        #endregion

        #region Properties

        public double DoubleProperty { get; }
        public int IntProperty { get; }
        public string StringProperty { get; }

        #endregion

        #region Constructors

        public ImmutableTest()
        {
        }

        #endregion
    }
    
    internal class Program
    {
        #region Methods

        private static void Main(string[] args)
        {
            var immutableTest1 = new ImmutableTest();
            var immutableTest2 = immutableTest1.Mutate()
                                               .WithDoubleField(1.0)
                                               .WithIntField(1)
                                               .WithStringField("1")
                                               .WithDoubleProperty(2.0)
                                               .Commit();
        }

        #endregion
    }
}
```
