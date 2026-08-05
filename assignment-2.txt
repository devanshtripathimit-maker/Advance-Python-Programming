# Decorator for formatting text
def bold_text(func):
    def wrapper(*args, **kwargs):
        text = func(*args, **kwargs)
        return f"*{text}*"
    return wrapper


# Report class
class Report:

    # Class variable to store templates
    templates = {}

    # Constructor
    def _init_(self, title, content):
        self.title = title
        self.content = content

    # Class method to add a template
    @classmethod
    def add_template(cls, name, template_func):
        cls.templates[name] = template_func

    # Class method to retrieve a template
    @classmethod
    def get_template(cls, name):
        return cls.templates.get(name)

    # Magic method to call a report instance
    def _call_(self, template_name):
        template = self.get_template(template_name)

        if template:
            return template(self)

        return "Template not found!"

    # String representation of the report
    def _str_(self):
        return f"Title: {self.title}\nContent: {self.content}"


# Simple template function
def simple_template(report):
    return (
        f"----- REPORT -----\n"
        f"Title: {report.title}\n"
        f"Content: {report.content}\n"
    )


# Fancy template function with bold formatting
@bold_text
def fancy_template(report):
    return (
        f"** FANCY REPORT **\n"
        f"Title: {report.title}\n"
        f"Content: {report.content}\n"
        f"*********"
    )


# Main function
def main():

    # Add templates to the Report class
    Report.add_template("simple", simple_template)
    Report.add_template("fancy", fancy_template)

    # Create a report instance
    report = Report(
        "Monthly Sales Report",
        "Sales increased by 20% this month."
    )

    # Generate reports using different templates
    print("Simple Template:\n")
    print(report("simple"))

    print("\nFancy Template:\n")
    print(report("fancy"))

    print("\nString Representation:\n")
    print(report)


# Run the program
if _name_ == "_main_":
    main()
