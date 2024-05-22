#include <iostream>
#include <chrono>
#include <ctime>

using namespace std;

using namespace chrono;


struct List
{
    int data;
    List* head;
    List* tail;
};

List* createList(int length)
{
    List* curr = 0,
        * next = 0;
    for (int i = 1; i <= length; ++i)
    {
        curr = new List;
        curr->data = rand() % 100;
        curr->tail = next;
        if (next)
            next->head = curr;
        next = curr;
    }
    curr->head = 0;
    return curr;
}

int* createMassive(int length) {
    int* Arr = new int[length];
    for (int i = 0; i < length; ++i)
        Arr[i]= rand() % 100;
    return Arr;
}

int* createBiggerMassive(int* Arr, int* length) {
    int* Arr2= new int[(*length)+1];
    for (int i = 0; i < (*length); i++) 
        Arr2[i] = Arr[i];
    delete[] Arr;
    return Arr2;
}

int* createMassive2(List*& beg) {
    int* Arr = new int[1];
    List* list = beg;
    int i = 0;
    int* p = &i;
    Arr[i] = list->data;
    list = list->tail;
    i++;
    while(list) {
        Arr = createBiggerMassive(Arr, p);
        Arr[i] = list->data;
        list = list->tail;
        i++;
    }
    return Arr;
}

List* createList2(int* col)
{
    time_point<steady_clock, duration<__int64, ratio<1, 1000000000>>> start, end;
    nanoseconds result;
    cout << "print numbers" << endl;
    int a;
    while (true) {
        if (!(cin >> a && a >= -2147483648 && a <= 2147483647))
        {
            cout << "Invalid input" << endl;
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            return 0;
        }
        else
            break;
    }
    start = steady_clock::now();
    (*col)++;
    List* curr = 0,
        * next = 0;
    curr = new List;
    curr->head = next;
    curr->data = a;
    next = curr;
    while (cin.peek() == ' ')
    {
        while (true) {
            if (!(cin >> a && a >= -2147483648 && a <= 2147483647))
            {
                cout << "Invalid input" << endl;
                cin.clear();
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
                return 0;
            }
            else
                break;
        }
        curr = new List;
        curr->head = next;
        curr->data = a;
        if (next)
            next->tail = curr;
        next = curr;
        (*col)++;
    }
    curr->tail = 0;
    while (curr->head)
        curr = curr->head;
    end = steady_clock::now();
    result = duration_cast<nanoseconds>(end - start);
    cout << "it has taken " << result.count() << " nanoseonds for spisok" << endl;
    return curr;
}

void deleteList(List*& beg)
{
    List* next;
    while (beg)
    {
        next = beg->tail;
        delete beg;
        beg = next;
    }
}

void changeList(List*& beg, int index1, int index2) {
    if (index1 > index2) {
        int g = index1;
        index1 = index2;
        index2 = g;
    }
    List* next;
    int currentindex = 0;
    List* beg1 = beg;
    List* beg2 = beg;
    while (currentindex != index2)
    {
        if (currentindex == index1)
            beg2 = beg1;
        next = beg1->tail;
        beg1 = next;
        currentindex++;
    }
    int g = beg1->data;
    beg1->data = beg2->data;
    beg2->data = g;
}

int findbyIndex(List*& beg, int index) {
    int currentIndex = 0;
    List* curr = beg;
    while (currentIndex < index)
    {
        curr = curr->tail;
        currentIndex++;
    }
    return curr->data;
}

int findbyData(List*& beg, int data) {
    int currentIndex = 0;
    int ColofFound = 0;
    int otwet;
    List* curr = beg;
    List* currnew = 0,
        * next = 0;
    while (curr)
    {
        if (curr->data == data) {
            currnew = new List;
            currnew->head = next;
            currnew->data = currentIndex;
            if (next)
                next->tail = currnew;
            next = currnew;
            ColofFound++;
        }
        curr = curr->tail;
        currentIndex++;
    }
    if (currnew) {
        currnew->tail = 0;
        while (currnew->head) {
            currnew = currnew->head;
        }
    }
    if (ColofFound == 0) {
        cout << "nothing was found" << endl;
        otwet = -1;
    }
    if (ColofFound == 1) {
        otwet = currnew->data;
    }
    if (ColofFound > 1) {
        cout << ColofFound << " elements was found with indexes:" << endl;
        next = currnew;
        while (next) {
            cout << next->data + 1 << " ";
            next = next->tail;
        }
        cout << endl;
        cout << "Choose one" << endl;
        next = currnew;
        int b = 0;
        while (true) {
            if (!(cin >> otwet && otwet >= 1 && otwet <= 2147483647))
            {
                cout << "Invalid input" << endl;
                cin.clear();
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
                cout << "Try again" << endl;
            }
            else {
                next = currnew;
                while (next) {
                    if (next->data + 1 == otwet) {
                        b = 1;
                        break;
                    }
                    next = next->tail;
                }
                if (b==1)
                    break;
                else
                    cout << "Invalid input" << endl << "Try again" << endl;
            }
        }
        otwet--;
    }
    if (ColofFound > 0) {
        deleteList(currnew);
    }
    return otwet;
}

List* listItem(List* beg, int index, bool errMsg = true)
{
    while (beg && (index--))
        beg = beg->tail;
    if (errMsg && !beg)
        cout << "Элемент списка отсутствует \n";
    return beg;
}

void delItem(List*& beg, int Index, int col)
{
    if (Index >= col) {
        return;
    }

    List* item;
    if (!Index) {
        item = beg->tail;
        delete beg;
        beg = item;
        beg->head = 0;
        return;
    }
    item = listItem(beg, Index - 1, 0);
    List* dItem = item->tail;
    item->tail = dItem->tail;
    if (item->tail)
        item->tail->head = item;
    delete dItem;
}

void cocktailSort(List*& beg, int n) {
    bool swapped = true;
    int start = 0;
    int end = n - 1;


    while (swapped) {
        swapped = false;

        for (int i = start; i < end; ++i) {
            if (findbyIndex(beg, i) > findbyIndex(beg, i + 1)) {
                changeList(beg, i, i + 1);
                swapped = true;
            }
        }

        if (!swapped) {
            break;
        }

        swapped = false;
        --end;

        for (int i = end - 1; i >= start; --i) {
            if (findbyIndex(beg, i) > findbyIndex(beg, i + 1)) {
                changeList(beg, i, i + 1);
                swapped = true;
            }
        }

        ++start;
    }
}

void changeData(List*& beg, int index, int data) {
    List* next = beg;
    int currentindex = 0;
    while (currentindex < index)
    {
        next = next->tail;
        currentindex++;
    }
    next->data = data;
}

List* insertData(List*& beg, int index, int data) {
    List* next = beg;
    List* curr = 0;
    int currentindex = 0;
    List* tailold;
    if (index > 0) {
        while (currentindex < index-1)
        {
            next = next->tail;
            currentindex++;
        }
        curr = new List;
        curr->data = data;
        curr->head = next;
        if (next->tail == 0) {
            next->tail = curr;
            curr->tail = 0;
            return beg;
        }
        tailold = next->tail;
        next->tail = curr;
        curr->tail = tailold;
        tailold->head = curr;
        return beg;
    }
    else {
        curr = new List;
        curr->data = data;
        curr->head = 0;
        curr->tail = next;
        next->head = curr;
        return curr;
    }
}

int* insertMassive(int* Arr, int index, int number, int col) {
    int* Arr2 = new int[col + 1];
    for (int i = 0; i < index; i++) {
        Arr2[i] = Arr[i];
    }
    Arr2[index] = number;
    if (index < col - 1) {
        for (int i = index + 1; i < col + 1; i++) {
            Arr2[i] = Arr[i - 1];
        }
    }
    delete[] Arr;
    return Arr2;
}

void seeSpisok(List*& beg) {
    List* curr = beg;
    while (curr)
    {

        cout << curr->data << ' ';
        curr = curr->tail;
    }
    cout << endl;
}

int findbyDataMassive(int* Arr, int data, int col, int index) {
    int ColofFound = 0;
    List* currnew = 0;
    List* next = 0;
    int otwet = 0;
    for (int i = 0; i < col; i++) {
        if (Arr[i] == data) {
            currnew = new List;
            currnew->head = next;
            currnew->data = i;
            if (next)
                next->tail = currnew;
            next = currnew;
            ColofFound++;
        }
    }
    if (currnew) {
        currnew->tail = 0;
        while (currnew->head) {
            currnew = currnew->head;
        }
    }
    if (ColofFound == 0) {
        otwet = -1;
    }
    if (ColofFound == 1) {
        otwet = currnew->data;
    }
    if (ColofFound > 1) {
        next = currnew;
        int b = 0;
        while (true) {
            otwet = index + 1;
            next = currnew;
            while (next) {
                if (next->data + 1 == otwet) {
                    b = 1;
                    break;
                }
                next = next->tail;
            }
            if (b == 1)
                break;
        }
        otwet--;
    }
    if (ColofFound > 0) {
        deleteList(currnew);
    }
    return otwet;
}

int* delItemMassive(int* Arr, int Index, int col){
    int* Arr2 = new int[col - 1];
    if (Index == 0) {
        for (int i = 0; i < col - 1; i++) {
            Arr2[i] = Arr[i + 1];
        }
    }
    else {
        for (int i = 0; i < Index; i++) {
            Arr2[i] = Arr[i];
        }
        if (Index < col) {
            for (int i = Index; i < col - 1; i++) {
                Arr2[i] = Arr[i + 1];
            }
        }
    }
    delete[] Arr;
    return Arr2;
}

void changeMassiveIndex(int* Arr, int index1, int index2) {
    int g = Arr[index1];
    Arr[index1] = Arr[index2];
    Arr[index2] = g;
}

void Massiveeualsspisok(int* Arr, List*& beg) {
    List* list = beg;
    int i = 0;
    while (list) {
        Arr[i] = list->data;
        list = list->tail;
        i++;
    }
}

void snakersort(int* B, int end2)
{
    bool swapped = true;
    int start2 = 0;
    while (swapped) {
        swapped = false;
        for (int i = start2; i < end2; ++i) {
            if (B[i] > B[i + 1]) {
                swap(B[i], B[i + 1]);
                swapped = true;
            }
        }
        if (!swapped) {
            break;
        }
        swapped = false;
        --end2;
        for (int i = end2 - 1; i >= start2; --i) {
            if (B[i] > B[i + 1]) {
                swap(B[i], B[i + 1]);
                swapped = true;
            }
        }
        ++start2;
    }
}

int main() {
    int* Arr = 0;
    time_point<steady_clock, duration<__int64, ratio<1, 1000000000>>> start, end;
    nanoseconds result;
    srand(time(0));
    int a;
    int col;
    int* colp = &col;
    int AorB;
    col = 0;
    bool go;
    int whatIndexorElement;
    int whatIndex2;
    go = true;
    List* list;
    list = 0;
    List* curr;
    curr = list;
    int what;
    while (go) {
        cout << endl << "What you want?" << endl << "0-exit" << endl << "1-create new spisok" << endl << "2-insert new element" << endl << "3-delete one element" << endl << "4-change 2 elements" << endl << "5-pick one element" << endl << "6-shaker sort(IDZ)" << endl << "7-see all data" << endl << endl;
        while (true) {
            if (!(cin >> what && what >= 0 && what <= 7))
            {
                cout << "Invalid input" << endl;
                cin.clear();
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
                cout << "Try again" << endl;
            }
            else
                break;
        }
        switch (what) {
        case(0):
            go = false;
            break;
        case(1):
            cout << "a or b?(1-a, 2-b (a-random numbers, b-your numbers))" << endl;
            while (true) {
                if (!(cin >> AorB && AorB >= 1 && AorB <= 2))
                {
                    cout << "Invalid input" << endl;
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    cout << "Try again" << endl;
                }
                else
                    break;
            }
            if (col > 0) {
                deleteList(list);
                delete[] Arr;
            }
            col = 0;
            if (AorB == 1) {
                cout << "how many?" << endl;
                if (!(cin >> col && col > 0 && col <= 2500000))
                {
                    cout << "Invalid input" << endl;
                    return 0;
                }
                start = steady_clock::now();
                list = createList(col);
                end = steady_clock::now();
                result = duration_cast<nanoseconds>(end - start);
                cout << "it has taken " << result.count() << " nanoseonds for spisok" << endl;
                start = steady_clock::now();
                Arr = createMassive(col);
                end = steady_clock::now();
                result = duration_cast<nanoseconds>(end - start);
                cout << "and " << result.count() << " nanoseonds for massive" << endl;
                Massiveeualsspisok(Arr, list);
            }
            else {
                list = createList2(colp);
                if (list == 0)
                    return 0;
                start = steady_clock::now();
                Arr = createMassive2(list);
                end = steady_clock::now();
                result = duration_cast<nanoseconds>(end - start);
                cout << "And " << result.count() << " nanoseonds for massive" << endl;
            }
            seeSpisok(list);
            cout << endl;
            break;
            //создание списка сделано
        case(2):
            if (col <= 0) {
                cout << "There are no elements in spisok" << endl;
                break;
            }
            cout << "print a number" << endl;
            while (true) {
                if (!(cin >> whatIndexorElement && whatIndexorElement >= -2147483648 && whatIndexorElement <= 2147483647))
                {
                    cout << "Invalid input" << endl;
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    cout << "Try again" << endl;
                }
                else
                    break;
            }
            cout << "print an index" << endl;
            while (true) {
                if (!(cin >> whatIndex2 && whatIndex2 >= 1 && whatIndex2 <= col + 1))
                {
                    cout << "Invalid input" << endl;
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    cout << "Try again" << endl;
                }
                else
                    break;
            }
            start = steady_clock::now();
            list = insertData(list, whatIndex2 - 1, whatIndexorElement);
            end = steady_clock::now();
            result = duration_cast<nanoseconds>(end - start);
            cout << "New element was added" << endl;
            cout << "it has taken " << result.count() << " nanoseonds for spisok" << endl;
            start = steady_clock::now();
            Arr = insertMassive(Arr, whatIndex2 - 1, whatIndexorElement, col);
            end = steady_clock::now();
            result = duration_cast<nanoseconds>(end - start);
            cout << "and " << result.count() << " nanoseonds for massive" << endl;
            //вставка сделана
            col++;
            seeSpisok(list);
            break;
        case(3):
            if (col <= 0) {
                cout << "There are no elements in spisok" << endl;
                break;
            }
            cout << "By element or index? (1-element, 2-index)" << endl;
            while (true) {
                if (!(cin >> AorB && AorB >= 1 && AorB <= 2))
                {
                    cout << "Invalid input" << endl;
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    cout << "Try again" << endl;
                }
                else
                    break;
            }
            a = 0;
            if (AorB == 1) {
                cout << "what element?" << endl;
                while (true) {
                    if (!(cin >> whatIndexorElement && whatIndexorElement >= -2147483648 && whatIndexorElement <= 2147483647))
                    {
                        cout << "Invalid input" << endl;
                        cin.clear();
                        cin.ignore(numeric_limits<streamsize>::max(), '\n');
                        cout << "Try again" << endl;
                    }
                    else
                        break;
                }
                start = steady_clock::now();
                a = findbyData(list, whatIndexorElement);
                if (a >= 0) {
                    delItem(list, a, col);
                    cout << "element deleted" << endl;
                    col--;
                }
                end = steady_clock::now();
                result = duration_cast<nanoseconds>(end - start);
                cout << "it has taken " << result.count() << " nanoseonds for spisok" << endl;
                start = steady_clock::now();
                a = findbyDataMassive(Arr, whatIndexorElement, col+1, a);
                if (a >= 0) {
                    Arr=delItemMassive(Arr, a, col+1);
                }
                end = steady_clock::now();
                result = duration_cast<nanoseconds>(end - start);
                cout << "And " << result.count() << " nanoseonds for massive" << endl;
            }
            else {
                cout << "what index?" << endl;
                while (true) {
                    if (!(cin >> whatIndexorElement && whatIndexorElement >= 1 && whatIndexorElement <= col))
                    {
                        cout << "Invalid input" << endl;
                        cin.clear();
                        cin.ignore(numeric_limits<streamsize>::max(), '\n');
                        cout << "Try again" << endl;
                    }
                    else
                        break;
                }
                start = steady_clock::now();
                delItem(list, whatIndexorElement - 1, col);
                cout << "element deleted" << endl;
                end = steady_clock::now();
                result = duration_cast<nanoseconds>(end - start);
                cout << "it has taken " << result.count() << " nanoseonds for spisok" << endl;
                start = steady_clock::now();
                Arr=delItemMassive(Arr, whatIndexorElement-1, col);
                col--;
                end = steady_clock::now();
                result = duration_cast<nanoseconds>(end - start);
                cout << "And " << result.count() << " nanoseonds for massive" << endl;
            }
            seeSpisok(list);
            break;
            //удаление сделано
        case(4):
            if (col <= 0) {
                cout << "There are no elements in spisok" << endl;
                break;
            }
            if (col == 1) {
                cout << "There are only one element in spisok" << endl;
                break;
            }
            cout << "what first index?" << endl;
            while (true) {
                if (!(cin >> whatIndexorElement && whatIndexorElement >= 1 && whatIndexorElement <= col))
                {
                    cout << "Invalid input" << endl;
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    cout << "Try again" << endl;
                }
                else
                    break;
            }
            cout << "what second index?" << endl;
            while (true) {
                if (!(cin >> whatIndex2 && whatIndex2 >= 1 && whatIndex2 <= col))
                {
                    cout << "Invalid input" << endl;
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    cout << "Try again" << endl;
                }
                else
                    break;
            }
            start = steady_clock::now();
            changeList(list, whatIndexorElement - 1, whatIndex2 - 1);
            cout << "elements changed" << endl;
            end = steady_clock::now();
            result = duration_cast<nanoseconds>(end - start);
            cout << "it has taken " << result.count() << " nanoseonds for spisok" << endl;
            start = steady_clock::now();
            changeMassiveIndex(Arr, whatIndexorElement - 1, whatIndex2 - 1);
            end = steady_clock::now();
            result = duration_cast<nanoseconds>(end - start);
            cout << "And " << result.count() << " nanoseonds for massive" << endl;
            seeSpisok(list);
            break;
            //обмен сделан
        case(5):
            if (col <= 0) {
                cout << "There are no elements in spisok" << endl;
                break;
            }
            cout << "By element or index? (1-element, 2-index)" << endl;
            while (true) {
                if (!(cin >> AorB && AorB >= 1 && AorB <= 2))
                {
                    cout << "Invalid input" << endl;
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    cout << "Try again" << endl;
                }
                else
                    break;
            }
            a = 0;
            if (AorB == 1) {
                cout << "what element?" << endl;
                while (true) {
                    if (!(cin >> whatIndexorElement && whatIndexorElement >= -2147483648 && whatIndexorElement <= 2147483647))
                    {
                        cout << "Invalid input" << endl;
                        cin.clear();
                        cin.ignore(numeric_limits<streamsize>::max(), '\n');
                        cout << "Try again" << endl;
                    }
                    else
                        break;
                }
                start = steady_clock::now();
                whatIndex2 = findbyData(list, whatIndexorElement);
                if (whatIndex2 < 0)
                    break;
                a = 1;
            }
            else {
                cout << "what index?" << endl;
                while (true) {
                    if (!(cin >> whatIndex2 && whatIndex2 >= 1 && whatIndex2 <= col))
                    {
                        cout << "Invalid input" << endl;
                        cin.clear();
                        cin.ignore(numeric_limits<streamsize>::max(), '\n');
                        cout << "Try again" << endl;
                    }
                    else
                        break;
                }
                whatIndex2--;
                start = steady_clock::now();
            }
            cout << "You choose element:" << findbyIndex(list, whatIndex2) << " with index:" << whatIndex2 + 1 << " (spisok)" <<endl;
            end = steady_clock::now();
            result = duration_cast<nanoseconds>(end - start);
            cout << "it has taken " << result.count() << " nanoseonds for spisok" << endl;
            start = steady_clock::now();
            if (a == 1)
                whatIndex2 = findbyDataMassive(Arr, whatIndexorElement, col, whatIndex2);
            cout << "You choose element:" << Arr[whatIndex2] << " with index:" << whatIndex2 + 1 << " (massive)" << endl;
            end = steady_clock::now();
            result = duration_cast<nanoseconds>(end - start);
            cout << "And " << result.count() << " nanoseonds for massive" << endl;
            cout << "Do you want to change its data? (1-yes, 2-no)" << endl;
            while (true) {
                if (!(cin >> AorB && AorB >= 1 && AorB <= 2))
                {
                    cout << "Invalid input" << endl;
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    cout << "Try again" << endl;
                }
                else
                    break;
            }
            if (AorB == 1) {
                cout << "Print new data" << endl;
                while (true) {
                    if (!(cin >> whatIndexorElement && whatIndexorElement >= -2147483648 && whatIndexorElement <= 2147483647))
                    {
                        cout << "Invalid input" << endl;
                        cin.clear();
                        cin.ignore(numeric_limits<streamsize>::max(), '\n');
                        cout << "Try again" << endl;
                    }
                    else
                        break;
                }
                changeData(list, whatIndex2, whatIndexorElement);
                Arr[whatIndex2] = whatIndexorElement;
                cout << "Data was changed" << endl;
            }
            seeSpisok(list);
            break;
            //получение сделано
        case(6):
            if (col <= 0) {
                cout << "There are no elements in spisok" << endl;
                break;
            }
            start = steady_clock::now();
            cocktailSort(list, col);
            cout << "spisok was sorted" << endl;
            end = steady_clock::now();
            result = duration_cast<nanoseconds>(end - start);
            cout << "it has taken " << result.count() << " nanoseonds for spisok" << endl;
            start = steady_clock::now();
            snakersort(Arr, col - 1);
            end = steady_clock::now();
            result = duration_cast<nanoseconds>(end - start);
            cout << "And " << result.count() << " nanoseonds for massive" << endl;
            seeSpisok(list);
            break;
            //сортировка
        case(7):
            cout << "Spisok:" << endl;
            seeSpisok(list);
            cout << "Massive:" << endl;
            for (int i = 0; i < col; i++) {
                cout << Arr[i] << " ";
            }
            cout << endl;
            
            break;
            //вывод сделан
        }
    }

    deleteList(list);
    delete[] Arr;
    return 0;

}
