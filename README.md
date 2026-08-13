import java.util.Scanner;

public class SmartHospitalBillingSystem {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("==================================================");
        System.out.println("           HOSPITAL BILLING INPUT ENTRY");
        System.out.println("==================================================");


        // --- 1. USER INPUT ---
        
        // Patient Information Inputs
        System.out.print("Enter Patient Name: ");
        String name = scanner.nextLine();
        
        System.out.print("Enter Patient ID: ");
        String id = scanner.nextLine();
        
        System.out.print("Enter Age: ");
        int age = scanner.nextInt();
        
        System.out.print("Enter Room Number: ");
        int rno = scanner.nextInt();
        
        int dc;        
        while (true) {            
            System.out.print("Enter Number of Days Confined (0 for outpatient): ");
 
            if (scanner.hasNextInt()) {
                dc = scanner.nextInt();

                if (dc >= 0) {
                    break;
                }

                System.out.println("Days confined cannot be negative.");
            } else {
                System.out.println("Please enter a whole number.");
                scanner.next();
            }
        }

        // Medical Diagnosis Code for automated insurance tracking (No Loop)
        System.out.print("Enter Diagnosis Code (DENGUE, PNEUMONIA, TYPHOID, APPENDECTOMY, ETC.): ");
        String dcode = scanner.next().toUpperCase();
           
        // --- 2. LOOP VALIDATION: ROOM RATE AUTO-SETTER ---
        // This loop forces a correct entry and will not allow ₱0.00 room rates
        double ratepday = 0.0;
        String type = "";


        while (ratepday == 0.0) {
            System.out.print("\nEnter Room Type (WARD, SEMI-PRIVATE, PRIVATE, VIP): ");
            type = scanner.next().toUpperCase();

            switch (type) {
                case "WARD":
                    ratepday = 1500.00;
                    System.out.println("\nWARD selected, daily rate = PHP 1,500.00\n");
                    break;
                case "SEMI-PRIVATE":
                    ratepday = 2500.00;
                    System.out.println("\nSEMI-PRIVATE selected, daily rate = PHP 2,500.00\n");
                    break;
                case "PRIVATE":
                    ratepday = 4000.00;
                    System.out.println("\nPRIVATE selected, daily rate = PHP 4,000.00\n");
                    break;
                case "VIP":
                    ratepday = 7500.00;
                    System.out.println("\nVIP selected, daily rate = PHP 7,500.00\n");
                    break;
                default:
                    ratepday = 0.00;       // Keeps rate at 0.0 to force the loop to repeat
                    System.out.println("\nInvalid room type. Please choose from the choices above.\n");
                    break;
            }
        }

        // --- 3. CONTINUATION OF STANDARD BILLING INPUTS ---
        System.out.print("Enter Number of Extra Persons (fee per day PHP 100. Additional PHP 100 per extra person): ");
        int numExtraPersons = scanner.nextInt();
        
        double extraPersonFeePerDay = 100.0; 

        // Medical Services Inputs
        System.out.print("Enter Doctor's Professional Fee: PHP ");
        double doctorFee = scanner.nextDouble();
        
        System.out.print("Enter Nursing Service Fee: PHP ");
        double nursingFee = scanner.nextDouble();
        
        System.out.print("Enter Laboratory Fee: PHP ");
        double laboratoryFee = scanner.nextDouble();
        
        System.out.print("Enter Medical Imaging Fee: PHP ");
        double imagingFee = scanner.nextDouble();
        
        System.out.print("Enter Medicine Cost: PHP ");
        double medicineCost = scanner.nextDouble();
        
        System.out.print("Enter Emergency Service Fee: PHP ");
        double emergencyFee = scanner.nextDouble();


        // Hospital Charges Inputs
        System.out.print("Enter Facility Fee (Room not included): PHP ");
        double facilityFee = scanner.nextDouble();
        
        System.out.print("Enter Administrative Fee: PHP ");
        double adminFee = scanner.nextDouble();
        
        System.out.print("Enter Medical Equipment Fee: PHP ");
        double equipmentFee = scanner.nextDouble();


        // Financial Information Inputs
        System.out.print("Enter Hospital Discount Percentage (e.g., 10 for 10%): ");
        double discountPercentage = scanner.nextDouble();
        
        System.out.print("Enter Cash Deposit Amount Paid: PHP ");
        double cashDeposit = scanner.nextDouble();


        // --- 4. INSURANCE AUTO-COVERAGE CALCULATION LAYER ---
        double calcInsuCove = 0.0;

        switch (dcode) {
            case "DENGUE":
                calcInsuCove = 16000.00;
                System.out.println("Dengue is selected, insurance amount = PHP 16,000.00\n");
                break;
            case "PNEUMONIA":
                calcInsuCove = 32000.00;
                System.out.println("Pneumonia is selected, insurance amount = PHP 32,000.00\n");
                break;
            case "TYPHOID":
                calcInsuCove = 19500.00;
                System.out.println("Typhoid is selected, insurance amount = PHP 19,500.0\n");
                break;
            case "APPENDECTOMY":
                calcInsuCove = 24000.00;
                System.out.println("Apendectomy is selected, insurance amount = PHP 24,000.00\n");
                break;
            default:
                calcInsuCove = 0.00;
                System.out.println("Diagnosis is not covered by insurance. Applied PHP 0.00 insurance.\n");
                break;
        }

        // --- 5. MATHEMATICAL CALCULATIONS SECTION ---

        // Room Expenses
        double basicRoomExpense = ratepday * dc;
        double extraPersonExpense = numExtraPersons * extraPersonFeePerDay * dc;
        double totalRoomExpenses = basicRoomExpense + extraPersonExpense;


        // Medical Expenses
        double totalMedicalExpenses = doctorFee + nursingFee + laboratoryFee + imagingFee + medicineCost + emergencyFee;


        // Hospital Expenses
        double totalHospitalExpenses = facilityFee + adminFee + equipmentFee;


        // Overall Hospital Bill
        double overallHospitalBill = totalRoomExpenses + totalMedicalExpenses + totalHospitalExpenses;


        // Discounts Calculation
        double discountAmount = overallHospitalBill * (discountPercentage / 100.0);
        double amountAfterDiscount = overallHospitalBill - discountAmount;


        // Insurance Allocation Safety Check
        double actualInsuranceCovered =
                Math.min(calcInsuCove, Math.max(0.0, amountAfterDiscount));
        
        // Final Remaining Bill Balances
        double patientFinancialResponsibility =
                Math.max(0.0, amountAfterDiscount - actualInsuranceCovered);

        double remainingBalance =
                Math.max(0.0, patientFinancialResponsibility - cashDeposit);


        // --- 6. FINANCIAL ANALYSIS CALCULATIONS ---


        double averageDailyCost =
                dc > 0 ? overallHospitalBill / dc : 0.0;
        
        double medicalExpensePercentage = 0.0;
        double roomExpensePercentage = 0.0;

        if (overallHospitalBill > 0) {
            medicalExpensePercentage =
                (totalMedicalExpenses / overallHospitalBill) * 100.0;
            roomExpensePercentage =
                (totalRoomExpenses / overallHospitalBill) * 100.0;
        }

        double insuranceCoveragePercentage = 0.0;
        
        if (amountAfterDiscount > 0) {
            insuranceCoveragePercentage = (actualInsuranceCovered / amountAfterDiscount) * 100.0;
        }

        // --- 7. LOGICAL ANALYSIS EXPRESSIONS ---

        boolean cond1_isHighBill = overallHospitalBill >= 50000.0;
        boolean cond2_isLongStay = dc > 5;
        boolean cond3_isInsuranceSufficient = calcInsuCove >= patientFinancialResponsibility; 
        boolean cond4_isBalanceZeroOrLess = remainingBalance <= 0.0;
        
        boolean isHighCostHospitalization = (overallHospitalBill >= 50000.0) && (dc > 5);
        boolean isInsuranceConsideredSufficient = (calcInsuCove >= patientFinancialResponsibility) && (remainingBalance <= 0.0);
        boolean isMedicalExpensesFiftyPercentOrMore = medicalExpensePercentage >= 50.0;




        // --- 8. REQUIRED FINAL OUTPUT REPORT DISPLAY ---


        System.out.println("\n\n==================================================");
        System.out.println(" HOSPITAL BILLING AND RESOURCE ANALYSIS REPORT ");
        System.out.println("==================================================");
        
        System.out.println("Patient Information");
        System.out.println(" • Patient Name: " + name);
        System.out.println(" • Patient ID: " + id);
        System.out.println(" • Age: " + age + " years old");
        System.out.println(" • Room Number: " + rno);
        System.out.println(" • Days Confined: " + dc);
        System.out.println(" • Medical Diagnosis: " + dcode);
        System.out.println(" • Room Type Category: " + type);
        System.out.println("--------------------------------------------------");


        System.out.println("Room Expense Summary");
        System.out.printf(" • Basic Room Expense: PHP %,.2f (Rate: PHP %,.2f/day)\n", basicRoomExpense, ratepday);
        System.out.printf(" • Extra Person Expense: PHP %,.2f\n", extraPersonExpense);
        System.out.printf(" • Total Room Expense: PHP %,.2f\n", totalRoomExpenses);
        System.out.println("--------------------------------------------------");


        System.out.println("Medical Expense Summary");
        System.out.printf(" • Total Medical Expenses: PHP %,.2f\n", totalMedicalExpenses);
        System.out.println("--------------------------------------------------");


        System.out.println("Hospital Expense Summary");
        System.out.printf(" • Total Hospital Expenses: PHP %,.2f\n", totalHospitalExpenses);
        System.out.println("--------------------------------------------------");

        System.out.println("Resource Analysis");
        System.out.printf(" • Average Daily Hosp. Cost:PHP %,.2f per day\n", averageDailyCost);
        System.out.printf(" • Medical Expense Percent: %.2f%%\n", medicalExpensePercentage);
        System.out.printf(" • Room Expense Percent: %.2f%%\n", roomExpensePercentage);
        System.out.printf(" • Insurance Cover Percent: %.2f%%\n", insuranceCoveragePercentage);

        System.out.println("--------------------------------------------------");
        System.out.println("Financial Summary");
        System.out.printf(" • Overall Hospital Bill: PHP %,.2f\n", overallHospitalBill);
        System.out.printf(" • Discount Amount: PHP %,.2f\n", discountAmount);
        System.out.printf(" • Amount After Discount: PHP %,.2f\n", amountAfterDiscount);
        System.out.printf(" • Insurance Coverage: PHP %,.2f\n", actualInsuranceCovered);
        System.out.printf(" • Patient Responsibility: PHP %,.2f\n", patientFinancialResponsibility);
        System.out.printf(" • Cash Deposit: PHP %,.2f\n", cashDeposit);
        System.out.printf(" • Remaining Balance: PHP %,.2f\n", remainingBalance);
        System.out.println("--------------------------------------------------");

        System.out.println("Analysis");
        System.out.println(" 1. Total hospital bill is PHP 50,000 or higher? " + cond1_isHighBill);
        System.out.println(" 2. Patient stayed for more than 5 days? " + cond2_isLongStay);
        System.out.println(" 3. Insurance coverage is sufficient after discount? " + cond3_isInsuranceSufficient);
        System.out.println(" 4. Remaining balance is zero or less? " + cond4_isBalanceZeroOrLess);
        System.out.println(" 5. Hospitalization is considered high-cost? " + isHighCostHospitalization);
        System.out.println(" 6. Insurance is considered sufficient? " + isInsuranceConsideredSufficient);
        System.out.println(" 7. Medical expenses represent 50% or more of overall bill? " + isMedicalExpensesFiftyPercentOrMore);
        System.out.println("==================================================");

        scanner.close();
    }
}
