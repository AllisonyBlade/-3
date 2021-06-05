# Lab3
// calculator operations
public enum CalcOperator
{
    None,
    Plus,
    Minus,
    Times,
    Divide
}


public partial class Calculator : Window
{
   //decimal separator of current culture
    char decimSepar = Convert.ToChar
        (CultureInfo.CurrentCulture.NumberFormat.NumberDecimalSeparator);

    private double a = 0;   //the first number
    private double b = 0;   //second number

   //last selected operation
    private CalcOperator lastOper = CalcOperator.None;

    public Calculator()
    {
        InitializeComponent();
    }

   //handle single digit (button)
    public void HandleDigit(int i)
    {
        string str = txtDisp.Text;

        if (lastOper == CalcOperator.None && a != Convert.ToDouble(str) ||
            lastOper != CalcOperator.None && b != Convert.ToDouble(str) ||
            str == "0")
        {
            str = string.Empty;
        }

        str += i.ToString();

        if (lastOper == CalcOperator.None)
            a = Convert.ToDouble(str);
        else
            b = Convert.ToDouble(str);

        txtDisp.Text = str;
    }

   //handle calculator operation (button)
    public void HandleOperator(CalcOperator oper)
    {
        txtDisp.Text = b.ToString();
        lastOper = oper;
    }

   //handle decimal separator selection (button)
    public void HandleDecimal()
    {
        if (!txtDisp.Text.Contains(decimSepar))
            txtDisp.Text += decimSepar;
    }

   //compute the result
    public void Compute()
    {
        double result = 0.0;

        switch (lastOper)
        {
            case CalcOperator.Plus:
                result = a + b;
                break;
            case CalcOperator.Minus:
                result = a - b;
                break;
            case CalcOperator.Times:
                result = a * b;
                break;
            case CalcOperator.Divide:
                if (b == 0.0)
                    result = 0.0;
                else
                    result = (double)a/b;
                break;
        }

        txtDisp.Text = result.ToString();
        lastOper = CalcOperator.None;
        a = 0;
        b = 0;
    }

    private void Window_Loaded(object sender, RoutedEventArgs e)
    {
        btnPlus.Tag = CalcOperator.Plus;
        btnMinus.Tag = CalcOperator.Minus;
        btnTimes.Tag = CalcOperator.Times;
        btnDivide.Tag = CalcOperator.Divide;
    }

    private void OnClickDigit(object sender, RoutedEventArgs e)
    {
        Button btn = sender as Button;
        HandleDigit(Convert.ToInt16(btn.Content.ToString()));
    }

    private void OnClickOperator(object sender, RoutedEventArgs e)
    {
        Button btn = sender as Button;
        HandleOperator((CalcOperator)btn.Tag);
    }

    private void btnEqual_Click(object sender, RoutedEventArgs e)
    {
        Compute();
    }

    private void btnDot_Click(object sender, RoutedEventArgs e)
    {
        HandleDecimal();
    }

}
