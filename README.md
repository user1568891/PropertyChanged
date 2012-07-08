## This is an add-in for [Fody](https://github.com/SimonCropp/Fody/) 

Injects [INotifyPropertyChanged](http://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx)  code into properties at compile time.

[Introduction to Fody](http://github.com/SimonCropp/Fody/wiki/SampleUsage)

## Nuget package http://nuget.org/packages/PropertyChanged.Fody 

### Your Code

    public class Person : INotifyPropertyChanged
    {
        public event PropertyChangedEventHandler PropertyChanged;

        public string GivenNames { get; set; }
        public string FamilyName { get; set; }

        public string FullName
        {
            get
            {
                return string.Format("{0} {1}", GivenNames, FamilyName);
            }
        }
    }


### What gets compiled

    public class Person : INotifyPropertyChanged
    {

        public event PropertyChangedEventHandler PropertyChanged;

        string givenNames;
        public string GivenNames
        {
            get { return givenNames; }
            set
            {
                if (value != givenNames)
                {
                    givenNames = value;
                    OnPropertyChanged("GivenNames");
                    OnPropertyChanged("FullName");
                }
            }
        }

        string familyName;
        public string FamilyName
        {
            get { return familyName; }
            set 
            {
                if (value != familyName)
                {
                    familyName = value;
                    OnPropertyChanged("FamilyName");
                    OnPropertyChanged("FullName");
                }
            }
        }

        public string FullName
        {
            get
            {
                return string.Format("{0} {1}", GivenNames, FamilyName);
            }
        }

        public virtual void OnPropertyChanged(string propertyName)
        {
            var propertyChanged = PropertyChanged;
            if (propertyChanged != null)
            {
                propertyChanged(this, new PropertyChangedEventArgs(propertyName));
            }
        }
    }
