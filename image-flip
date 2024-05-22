import cv2

def get_contours(image, inv_bool):
    grayim = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
    #cv2.imshow("gray", grayim) 
    if inv_bool == True:
        idk, blackim = cv2.threshold(grayim, 5, 100, cv2.THRESH_BINARY_INV)
        #cv2.imshow("black", blackim) 
    else: 
        idk, blackim = cv2.threshold(grayim, 5, 100, cv2.THRESH_BINARY)
        #cv2.imshow("black", blackim) 
    contours,hierarchy = cv2.findContours(blackim, 1,  cv2.CHAIN_APPROX_SIMPLE)
    #cv2.drawContours(image, contours, -1, (0, 0, 255), 5)
    #cv2.imshow("skrrt", image)
    return contours

def biggest_area(contours):
    max_area = 0
    max_cnt = None
    for i in contours:
        cnt_area = cv2.contourArea(i)
        if cnt_area >= max_area:
            max_area = cnt_area
            max_cnt = i
    return max_cnt

def sec_biggest_area(contours):
    max_area = 0
    max_cnt = None
    sec_cnt = None
    for i in contours:
        cnt_area = cv2.contourArea(i)
        if cnt_area > max_area:
            sec_cnt = max_cnt
            max_area = cnt_area
            max_cnt = i
    return sec_cnt

def least_corners(contours):
    min_corn = 100
    min_cnt = contours[0]
    for cc in contours:
        if (3 < len(cc) < min_corn):
            min_corn = len(cc)
            min_cnt = cc
        elif len(cc) == min_corn:
            if cv2.contourArea(cc) > cv2.contourArea(min_cnt):
                min_corn = len(cc)
                min_cnt = contours[i]
    return min_cnt

def sl_corners(contours):
    min_corn = len(contours[0])
    min_cnt = contours[0]
    sec_min_cnt = contours[0]
    for cc in contours:
        if len(cc) > 3:
            if len(cc) < min_corn:
                min_corn = len(cc)
                sec_min_cnt = min_cnt
                min_cnt = cc
            if len(cc) == min_corn:
                if cv2.contourArea(cc) > cv2.contourArea(min_cnt):
                    min_corn = len(cc)
                    sec_min_cnt = min_cnt
                    min_cnt = cc
    return sec_min_cnt

def sg_corners(contour):
    y_max = 0
    y_min = contour[0][0][1]
    x_max = 0
    x_min = contour[0][0][0]
    for i in range(len(contour)):
        y_val = contour[i][0][1]
        x_val = contour[i][0][0]
        #print(x_val)
        #print(y_val, x_val)
        if y_val > y_max:
            y_max = y_val
        if y_val < y_min:
            y_min = y_val
        if x_val > x_max:
            x_max = x_val
        if x_val < x_min:
            x_min = x_val
    #print(y_min, y_max, x_min, x_max)
    return (y_min, y_max, x_min, x_max)

def db_corners(left, right):
    #min y, min x from left
    #max y, max x from right
    y_min = left[0][0][1]
    x_min = left[0][0][0]
    for i in range(len(left)):
        y_val = left[i][0][1]
        x_val = left[i][0][0]
        #print(x_val)
        #print(y_val, x_val)
        if y_val < y_min:
            y_min = y_val
        if x_val < x_min:
            x_min = x_val
    y_max = 0
    x_max = 0
    for i in range(len(right)):
        y_val = right[i][0][1]
        x_val = right[i][0][0]
        if y_val > y_max:
            y_max = y_val
        if x_val > x_max:
            x_max = x_val
    #print(y_min, y_max, x_min, x_max)
    return (y_min, y_max, x_min, x_max)

def db_corners2(left, right):
    ltup = sg_corners(left)
    rtup = sg_corners(right)
    cortup = []
    cortup.append(min(ltup[0], rtup[0]))
    cortup.append(max(ltup[1], rtup[1]))
    cortup.append(min(ltup[2], rtup[2]))
    cortup.append(max(ltup[3], rtup[3]))
    return tuple(cortup)

def im_from_tup(image, tp):
    return image[tp[0]:tp[1], tp[2]:tp[3]]

image = cv2.imread("doudingle3.png")
image = cv2.resize(image, None, fx= 0.4, fy= 0.4, interpolation= cv2.INTER_LINEAR)

black_cnt = get_contours(image, True)
#cv2.drawContours(image, video_cnt, -1, (100, 0, 255), 2)
#cv2.imshow("video_cnt", image)
#print(len(video_cnt))

blackleft = biggest_area(black_cnt)
blackright = sec_biggest_area(black_cnt)
#print(len(blackleft))
#print(len(blackright))
#cv2.drawContours(image, blackleft, -1, (200, 0, 0), 20)
#cv2.drawContours(image, blackright, -1, (200, 200, 0), 20)
cv2.imshow("blocks", image)

vid_cnt = db_corners2(blackleft, blackright)
# print(vid_cnt)
vid_im = im_from_tup(image, vid_cnt)
cv2.imshow("faces", vid_im)

print("whee, first commit")
faces_cnt = get_contours(vid_im, False)
print(len(faces_cnt))
# #cv2.drawContours(vid_im, faces_cnt, -1, (200, 200, 0), 2)
# #cv2.imshow("faces", vid_im)
a_cnt = biggest_area(faces_cnt)
print(len(a_cnt))
a_tup = sg_corners(a_cnt)
#cv2.drawContours(vid_im, a_cnt, -1, (200, 0, 200), 5)
cv2.imshow("face", vid_im)
alyx = im_from_tup(vid_im, a_tup)
cv2.imshow("she!!", alyx)
flipped = cv2.flip(alyx, 0)
vid_im[a_tup[0]:a_tup[1], a_tup[2]:a_tup[3]] = flipped
image[vid_cnt[0]:vid_cnt[1], vid_cnt[2]:vid_cnt[3]] = vid_im
cv2.imshow("your face is upside down", image)

cv2.waitKey(0)
cv2.destroyAllWindows()
