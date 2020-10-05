# Burgess-exercise6-167
Calculate a users total price with discounts and taxes by incorporating methods, repetition structures, and selection structures to reduce complexity. Along with a receipt at the end.
package edu.CPT167.burgess.exercise6;
import java.util.Scanner;
public class BurgessMainClass 
{
	//AUTHOR: VICTOR BURGESS
	//COURSE: CPT 167
	/* Calculate a users total price with discounts and taxes by incorporating methods, repetition structures, and selection structures
	 *  to reduce complexity, along with having the count of discounts and items at the end of the program.
	 */

	//CONSTANT DECLARATION
	public static final double TAX_RATE = 0.075;
	public static final String DISCOUNT_NAME_MEMBER = "Member";
	public static final String DISCOUNT_NAME_SENIOR = "Senior";
	public static final String DISCOUNT_NAME_NONE = "No Discount";
	public static final String DISCOUNT_NAME_QUIT = "Quit";
	public static final double DISCOUNT_RATE_MEMBER = 0.15;
	public static final double DISCOUNT_RATE_SENIOR = 0.25;
	public static final double DISCOUNT_RATE_NONE = 0.00;
	public static final String ITEM_NAME_PREMIUM = "Omega";
	public static final String ITEM_NAME_SPECIAL = "Beta";
	public static final String ITEM_NAME_BASIC = "Alpha";
	public static final String ITEM_NAME_RETURN = "Return to Main Menu";
	public static final double ITEM_PRICE_PREMIUM = 55.95;
	public static final double ITEM_PRICE_SPECIAL = 24.95;
	public static final double ITEM_PRICE_BASIC = 15.95;

	public static void main(String[] args) 
	{
		Scanner input = new Scanner(System.in);

		//VARIABLE DECLARATION
		String userName = "";
		char rateSelection = ' ';
		char itemSelection = ' ';
		int itemQuantity = 0;
		String discountName = "";
		double discountRate = 0.00;
		String itemName = "";
		double itemPrice= 0.00;
		double discountAmount = 0.00;
		double discountPrice = 0.00;
		double subTotal = 0.00;
		double tax = 0.00;
		double totalCost = 0.00;
		int memberCount = 0;
		int seniorCount = 0;
		int noDiscountCount = 0;
		double grandTotal = 0.00;
		int premiumCount = 0;
		int specialCount = 0;
		int basicCount = 0;
		double purchaseAmount = 0.00;

		//Show the welcome banner
		displayWelcomeBanner();

		userName = getUserName(input);
		rateSelection = validateMainMenu(input);

		while (rateSelection !=  'Q')
		{
			itemSelection = validateItemMenu(input);

			while (itemSelection != 'R')
			{
				itemQuantity = validateItemQuantity(input);

				//DETERMINE discount name and rate using selection structure.
				if (rateSelection == 'A')
				{
					discountName = DISCOUNT_NAME_MEMBER;
					discountRate = DISCOUNT_RATE_MEMBER;
					memberCount++;
				}

				else if (rateSelection == 'B')
				{
					discountName = DISCOUNT_NAME_SENIOR;
					discountRate = DISCOUNT_RATE_SENIOR;
					seniorCount++;
				}

				else
				{
					discountName = DISCOUNT_NAME_NONE;
					discountRate = DISCOUNT_RATE_NONE;
					noDiscountCount++;
				}

				//Determine itemName and itemPrice using selectionStructure
				if (itemSelection == 'A')
				{
					itemName = ITEM_NAME_PREMIUM;
					itemPrice = ITEM_PRICE_PREMIUM;
					premiumCount++;
				}

				else if (itemSelection == 'B')
				{
					itemName = ITEM_NAME_SPECIAL;
					itemPrice = ITEM_PRICE_SPECIAL;
					specialCount++;
				}

				else
				{
					itemName = ITEM_NAME_BASIC;
					itemPrice = ITEM_PRICE_BASIC;
					basicCount++;
				}

				//Calculations
				discountAmount = itemPrice * discountRate;
				discountPrice = itemPrice - discountAmount;
				purchaseAmount = itemQuantity * discountPrice;
				subTotal = subTotal + purchaseAmount;

				displayItemReceipt(itemName, itemPrice, discountName, discountRate, discountAmount, discountPrice, itemQuantity, purchaseAmount, subTotal);

				itemSelection = validateItemMenu(input);

			}

			//Calculations +
			tax = subTotal * TAX_RATE;
			totalCost = subTotal + tax;
			grandTotal = grandTotal + totalCost;

			displayOrderTotal(userName, subTotal, tax, totalCost);

			//Reset subTotal
			subTotal = 0.00;

			rateSelection = validateMainMenu(input);

		}

		if (grandTotal > 0.00)
		{
			displayFinalReport(memberCount, seniorCount, noDiscountCount, premiumCount, specialCount, basicCount, grandTotal);
		}



	}

	//Welcome Banner
	public static void displayWelcomeBanner()
	{
		System.out.println("~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~");
		System.out.println("Welcome to THE NEW AND IMPROVED Home Depot's Sales!");
		System.out.println("This program allows you to select any item you want");
		System.out.println("along with it's combo And calculate it's total with our discount!");
		System.out.println("~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~");
	}


	public static String getUserName(Scanner borrowedInput)
	{
		String localUserName = "";
		System.out.println("First of all, what's your name?");
		localUserName = borrowedInput.nextLine();
		return localUserName;
	}
	
	public static void displayMainMenu()
	{
		System.out.println("~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~");
		System.out.println("DISCOUNT MENU");
		System.out.printf("%-8s%-15s%3.1f%1s\n", "[A] for",DISCOUNT_NAME_MEMBER,DISCOUNT_RATE_MEMBER*100,"%" );
		System.out.printf("%-8s%-15s%3.1f%1s\n", "[B] for",DISCOUNT_NAME_SENIOR,DISCOUNT_RATE_SENIOR*100,"%" );
		System.out.printf("%-8s%-15s%3.1f%1s\n", "[C] for",DISCOUNT_NAME_NONE,DISCOUNT_RATE_NONE*100,"%" );
		System.out.printf("%-7s%-20s\n", "[Q] to",DISCOUNT_NAME_QUIT);
		System.out.println("~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~");
		System.out.println("Please select the discount you qualify for");
	}

	public static char validateMainMenu(Scanner borrowedInput)
	{
		char localSelection = ' ';
		//DISCOUNT SELECTION
		displayMainMenu();
		localSelection = borrowedInput.next().toUpperCase().charAt(0);

		//Test for error using repetition structure
		while (localSelection != 'A' && localSelection != 'B' && localSelection != 'C' && localSelection != 'Q')
		{
			System.out.println("Invalid option, try again.");
			displayMainMenu();
			localSelection = borrowedInput.next().toUpperCase().charAt(0);
		}
		return localSelection;
	}
	
	public static void displayItemMenu()
	{
		System.out.println("~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~");
		System.out.println("ITEM MENU");
		System.out.printf("%-8s%-9s%5s%6.2f\n","[A] for",ITEM_NAME_PREMIUM,"$",ITEM_PRICE_PREMIUM);
		System.out.printf("%-8s%-9s%5s%6.2f\n","[B] for",ITEM_NAME_SPECIAL,"$",ITEM_PRICE_SPECIAL);
		System.out.printf("%-8s%-9s%5s%6.2f\n","[C] for",ITEM_NAME_BASIC,"$",ITEM_PRICE_BASIC);
		System.out.printf("%-7s%-20s\n", "[R] to", ITEM_NAME_RETURN);
		System.out.println("~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~");
		System.out.println("Enter your selection here");
	}

	public static char validateItemMenu(Scanner borrowedInput)
	{
		char localSelection = ' ';
		//Item Menu Selection
		
		
		displayItemMenu();
		localSelection = borrowedInput.next().toUpperCase().charAt(0);
		while (localSelection != 'A' && localSelection != 'B' && localSelection != 'C' && localSelection != 'R')
		{
			System.out.println("Invalid option, try again");
			displayItemMenu();
			localSelection = borrowedInput.next().toUpperCase().charAt(0);
		}

		return localSelection;
	}

	public static int validateItemQuantity(Scanner borrowedInput)
	{
		int localItemQuantity = 0;
		System.out.println("How many do you want?");
		localItemQuantity = borrowedInput.nextInt();

		while (localItemQuantity <= 0)
		{
			System.out.println("Invalid answer, try again");
			System.out.println("How many do you want?");
			localItemQuantity = borrowedInput.nextInt();
		}
		return localItemQuantity;
	}

	public static void displayItemReceipt(String borrowedItemName, double borrowedItemPrice, String borrowedDiscountName, double borrowedDiscountRate, double borrowedDiscountAmount, double borrowedDiscountPrice, int borrowedItemQuantity, double borrowedPurchaseAmount, double borrowedSubTotal) 
	{
		System.out.println("\n~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~");
		System.out.println("Item Report");
		System.out.println("~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~");
		System.out.printf("%-20s%2s%2s\n","Item Name","",borrowedItemName);
		System.out.printf("%-20s%3s%9.2f\n","Item Price","$",borrowedItemPrice);
		System.out.printf("%-22s%-20s\n", "Discount Type", borrowedDiscountName);
		System.out.printf("%-20s%11.1f%2s\n","Discount",(borrowedDiscountRate)*100, "%");
		System.out.printf("%-20s%3s%9.2f\n", "Discounted Amount","$",borrowedDiscountAmount);
		System.out.printf("%-20s%3s%9.2f\n","Discounted Price","$", borrowedDiscountPrice);
		System.out.printf("%-20s%2s%10d\n","Quantity","",borrowedItemQuantity);
		System.out.printf("%-20s%3s%9.2f\n","Purchase Amount","$", borrowedPurchaseAmount);
		System.out.printf("%-20s%3s%9.2f\n","Subtotal","$", borrowedSubTotal);
		System.out.println("~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~");
		System.out.println();
	}

	public static void displayOrderTotal(String borrowedUserName, double borrowedSubTotal, double borrowedTax, double borrowedTotalCost)
	{
		System.out.println("\n~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~");
		System.out.println("Order Report");
		System.out.println("~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~");
		System.out.printf("%-20s%2s%2s\n","Name","",borrowedUserName);
		System.out.printf("%-20s%3s%9.2f\n","Subtotal","$", borrowedSubTotal);
		System.out.printf("%-20s%3s%9.2f\n","Tax","$", borrowedTax);
		System.out.printf("%-20s%3s%9.2f\n", "Total Cost","$", borrowedTotalCost);
		System.out.println("~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~");

	}

	public static void displayFinalReport(int borrowedMemberCount, int borrowedSeniorCount, int borrowedNoDiscountCount, int borrowedPremiumCount, int borrowedSpecialCount, int borrowedBasicCount, double borrowedGrandTotal)
	{
		System.out.println("~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~");
		System.out.println("FINAL REPORT");
		System.out.println("~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~");
		System.out.println("Count of discount types:");
		System.out.printf("%-20s%4d\n", DISCOUNT_NAME_MEMBER,borrowedMemberCount);
		System.out.printf("%-20s%4d\n", DISCOUNT_NAME_SENIOR,borrowedSeniorCount);
		System.out.printf("%-20s%4d\n", DISCOUNT_NAME_NONE,borrowedNoDiscountCount);
		System.out.println("~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~");
		System.out.println("Count of item types");
		System.out.printf("%-20s%4d\n", ITEM_NAME_PREMIUM, borrowedPremiumCount);
		System.out.printf("%-20s%4d\n", ITEM_NAME_SPECIAL, borrowedSpecialCount);
		System.out.printf("%-20s%4d\n", ITEM_NAME_BASIC, borrowedBasicCount);
		System.out.println("~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~");
		System.out.printf("%-20s%2s%7.2f\n", "Grand Total","$",borrowedGrandTotal);
	}

	public static void displayFareWellMessage()
	{
		System.out.println("~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~");
		System.out.println("Thanks for using IMPROVED Home Depot Sales");
		System.out.println("We hope you have a great day!");
		System.out.println("~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~ ~~~~");
	}
}
