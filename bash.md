## from replit

```
#!/bin/bash

# Function to calculate discount/interest
calculate_discount_interest() {
  price=$1
  rate=$2
  echo $(echo "scale=2; $price * $rate / 100" | bc)
}

echo "Sales Input:"
read -p "Enter Product 1: " product1
read -p "Enter Price: " price1
read -p "Enter Product 2: " product2
read -p "Enter Price: " price2
read -p "Enter Product 3: " product3
read -p "Enter Price: " price3

read -p "Input Mode of Payment Terms
Cash Discount: " cash_discount
echo "You input $cash_discount% for Cash Discount"
read -p "3 Months Installment: " installment_3
echo "You input $installment_3% for 3 Months Installment"
read -p "6 Months Installment: " installment_6
echo "You input $installment_6% for 6 Months Installment"
read -p "12 Months Installment: " installment_12
echo "You input $installment_12% for 12 Months Installment"

echo "Main Menu"
echo "Press I: $product1 $price1"
echo "Press S: $product2 $price2"
echo "Press X: $product3 $price3"
read -p "Enter Item: " item

case $item in
  I)
    selected_product=$product1
    price=$price1
    ;;
  S)
    selected_product=$product2
    price=$price2
    ;;
  X)
    selected_product=$product3
    price=$price3
    ;;
  *)
    echo "Invalid selection"
    exit 1
    ;;
esac

echo "Your Item is $selected_product"
echo "Price: $price"

read -p "Select Mode of Payment
Press 1: Cash $cash_discount% Discount
Press 2: 3 Months Installment $installment_3% Interest
Press 3: 6 Months Installment $installment_6% Interest
Press 4: 12 Months Installment $installment_12% Interest
Enter Mode of Payment: " payment_mode

case $payment_mode in
  1)
    discount=$(calculate_discount_interest $price $cash_discount)
    echo "Mode of Payment: Cash Discount $cash_discount%"
    ;;
  2)
    interest=$(calculate_discount_interest $price $installment_3)
    total_amount=$(echo "scale=2; $price + $interest" | bc)
    echo "Mode of Payment: 3 Months Installment"
    ;;
  3)
    interest=$(calculate_discount_interest $price $installment_6)
    total_amount=$(echo "scale=2; $price + $interest" | bc)
    echo "Mode of Payment: 6 Months Installment"
    ;;
  4)
    interest=$(calculate_discount_interest $price $installment_12)
    total_amount=$(echo "scale=2; $price + $interest" | bc)
    echo "Mode of Payment: 12 Months Installment"
    ;;
  *)
    echo "Invalid payment mode"
    exit 1
    ;;
esac

echo "Item: $selected_product"
echo "Price: $price"
if [ ! -z $discount ]; then
  echo "Discount: $discount"
else
  echo "Interest: $interest"
fi
echo "Total Amount: $total_amount"
monthly_payment=$(echo "scale=2; $total_amount / 3" | bc)
echo "Monthly: $monthly_payment"

# Save this bash script as Cellphone.sh
# Generate the PDF of this script for documentation
# Record a video demonstrating the input and output scenarios as mentioned in the task
```


## from gemini
```
#!/bin/bash

# Function to display the main menu
display_menu() {
  echo "Sales Input:"
  echo ""
  echo -n "Enter Product 1: "
  read product1
  echo -n "Enter Price: $"
  read price1

  echo -n "Enter Product 2: "
  read product2
  echo -n "Enter Price: $"
  read price2

  echo -n "Enter Product 3: "
  read product3
  echo -n "Enter Price: $"
  read price3

  echo ""
  echo "Input Mode of Payment Terms:"
  echo ""
  echo "Cash Discount: 10"
  echo "3 Months Installment: 5"
  echo "6 Months Installment: 10"
  echo "12 Months Installment: 20"
  echo ""
}

# Function to calculate discount/interest based on mode of payment
calculate_amount() {
  local price=$1
  local mode=$2
  local discount=0
  local interest=0

  case $mode in
    1)
      discount=$((price * 10 / 100))
      ;;
    2)
      interest=$((price * 5 / 100))
      ;;
    3)
      interest=$((price * 10 / 100))
      ;;
    4)
      interest=$((price * 20 / 100))
      ;;
    *)
      echo "Invalid mode of payment."
      exit 1
      ;;
  esac

  local total_amount=$((price + interest - discount))
  local monthly_payment=0

  if [[ $mode -gt 1 ]]; then
    monthly_payment=$((total_amount / ((mode - 1) * 3)))
  fi

  echo "$discount $interest $total_amount $monthly_payment"
}

# Main program flow
clear

display_menu

# Get user input for main menu
echo "Main Menu:"
echo ""
echo "Press I: $product1 ($price1)"
echo "Press S: $product2 ($price2)"
echo "Press X: $product3 ($price3)"
echo -n "Enter Item: "
read item

# Validate user input
if [[ ! "$item" =~ ^[ISX]$ ]]; then
  echo "Invalid item selection."
  exit 1
fi

# Display item details based on user selection
case $item in
  I)
    echo "Your Item is $product1"
    echo "Price: $$price1"
    ;;
  S)
    echo "Your Item is $product2"
    echo "Price: $$price2"
    ;;
  X)
    echo "Your Item is $product3"
    echo "Price: $$price3"
    ;;
esac

# Get user input for mode of payment
echo ""
echo "Select Mode of Payment:"
echo ""
echo "Press 1: Cash (10% Discount)"
echo "Press 2: 3 Months Installment (5% Interest)"
echo "Press 3: 6 Months Installment (10% Interest)"
echo "Press 4: 12 Months Installment (20% Interest)"
echo -n "Enter Mode of Payment: "
read mode

# Calculate discount/interest and total amount
amount_details=$(calculate_amount $price1 $mode)  # Assuming price1 is used for calculation

# Display final output
discount=$(echo $amount_details | cut -d ' ' -f 1)
interest=$(echo $amount_details | cut -d ' ' -f 2)
total_amount=$(echo $amount_details | cut -d ' ' -f 3)
monthly_payment=$(echo $amount_details | cut -d ' ' -f 4)

echo "Mode of Payment: $((mode - 1)) Months Installment"  # Adjust for Cash discount display
[[ $mode -gt 1 ]] && echo "Discount/Interest: $$discount"  # Display discount only for installments
[[ $mode -gt 1 ]] && echo "Interest: $$interest"  # Display interest only for installments
echo "Total Amount: $$total_amount"
[[ $mode -gt 1 ]] && echo "Monthly: $$monthly_payment"

echo ""
echo "** Thank you for shopping! **"

```

## from chatgpt
```
#!/bin/bash

# Function to calculate the total amount and monthly payment
calculate_payment() {
  local price=$1
  local discount_rate=$2
  local months=$3
  local discount_or_interest
  local total_amount
  local monthly_payment

  discount_or_interest=$(echo "$price * $discount_rate / 100" | bc -l)
  if [[ $months -eq 1 ]]; then
    total_amount=$(echo "$price - $discount_or_interest" | bc -l)
  else
    total_amount=$(echo "$price + $discount_or_interest" | bc -l)
  fi
  monthly_payment=$(echo "$total_amount / $months" | bc -l)
  echo "$discount_or_interest $total_amount $monthly_payment"
}

# Input Sales
echo "Sales Input:"
echo -n "Enter Product 1: "
read product1
echo -n "Enter Price: "
read price1
price1=$(echo $price1 | tr -d '$,')

echo -n "Enter Product 2: "
read product2
echo -n "Enter Price: "
read price2
price2=$(echo $price2 | tr -d '$,')

echo -n "Enter Product 3: "
read product3
echo -n "Enter Price: "
read price3
price3=$(echo $price3 | tr -d '$,')

# Input Mode of Payment Terms
echo "Input Mode of Payment Terms"
echo -n "Cash Discount: "
read cash_discount
echo "You input ${cash_discount}% for Cash Discount"

echo -n "3 Months Installment: "
read three_months_installment
echo "You input ${three_months_installment}% for 3 Months Installment"

echo -n "6 Months Installment: "
read six_months_installment
echo "You input ${six_months_installment}% for 6 Months Installment"

echo -n "12 Months Installment: "
read twelve_months_installment
echo "You input ${twelve_months_installment}% for 12 Months Installment"

# Main Menu
echo "Main Menu"
echo "Press I: $product1 \$$price1"
echo "Press S: $product2 \$$price2"
echo "Press X: $product3 \$$price3"

echo -n "Enter Item: "
read item

case $item in
  I)
    selected_item=$product1
    selected_price=$price1
    ;;
  S)
    selected_item=$product2
    selected_price=$price2
    ;;
  X)
    selected_item=$product3
    selected_price=$price3
    ;;
  *)
    echo "Invalid Item"
    exit 1
    ;;
esac

echo "Your Item is $selected_item"
echo "Price: \$$selected_price"

# Select Mode of Payment
echo "Select Mode of Payment"
echo "Press 1: Cash ${cash_discount}% Discount"
echo "Press 2: 3 Months Installment ${three_months_installment}% Interest"
echo "Press 3: 6 Months Installment ${six_months_installment}% Interest"
echo "Press 4: 12 Months Installment ${twelve_months_installment}% Interest"

echo -n "Enter Mode of Payment: "
read payment_mode

case $payment_mode in
  1)
    mode_of_payment="Cash"
    discount_rate=$cash_discount
    months=1
    ;;
  2)
    mode_of_payment="3 Months Installment"
    discount_rate=$three_months_installment
    months=3
    ;;
  3)
    mode_of_payment="6 Months Installment"
    discount_rate=$six_months_installment
    months=6
    ;;
  4)
    mode_of_payment="12 Months Installment"
    discount_rate=$twelve_months_installment
    months=12
    ;;
  *)
    echo "Invalid Mode of Payment"
    exit 1
    ;;
esac

# Calculate Discount/Interest, Total Amount and Monthly Payment
result=$(calculate_payment $selected_price $discount_rate $months)
discount_or_interest=$(echo $result | awk '{print $1}')
total_amount=$(echo $result | awk '{print $2}')
monthly_payment=$(echo $result | awk '{print $3}')

# Output Results
echo "Mode of Payment: $mode_of_payment"
echo "Item: $selected_item"
echo "Price: \$$selected_price"
if [[ $months -eq 1 ]]; then
  echo "Discount: \$$discount_or_interest"
else
  echo "Interest: \$$discount_or_interest"
fi
echo "Total Amount: \$$total_amount"
if [[ $months -ne 1 ]]; then
  echo "Monthly: \$$monthly_payment"
fi

```
